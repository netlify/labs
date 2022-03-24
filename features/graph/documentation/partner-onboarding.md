# Partner onboarding requirements for Netlify Graph

This document outlines the requirements for partners to integrate with [Netlify Graph](https://www.netlify.com/blog/announcing-netlify-graph-a-faster-way-for-teams-to-develop-web-apps-with-apis), which enables two key components for their customers:

1. API Authentication
2. Graph API/Explorer integration

Both components can be integrated separately. However, we recommend integrating with both as they work best in tandem.

## API Authentication

The [API Authentication experience](authentication.md) enables developers to easily build workflows that use partner APIs without writing the authentication code from scratch. By abstracting the OAuth complexity, developers can log in to the partner service with Netlify Graph and re-use the partner API token whenever necessary. No need to worry about refreshes, whether the right scopes are selected, or whether the user needs to be re-authenticated - it's all handled automatically by Netlify Graph.

### Endpoints

We require API partners to implement the [Open ID Connect (OIDC)](https://openid.net/connect/) standard. OIDC can be thought of as an extension to the OAuth2 standard and it's feasible to build OIDC features on top of a working OAuth2 setup.

In practical terms, this means implementing the following endpoints:

- [Discovery endpoint](https://openid.net/specs/openid-connect-discovery-1_0.html): a JSON endpoint describing metadata about the location of endpoints described in this section.
    - Google implementation example: [`https://accounts.google.com/.well-known/openid-configuration`](https://accounts.google.com/.well-known/openid-configuration)
    - Spotify implementation example: [`https://accounts.spotify.com/.well-known/openid-configuration`](https://accounts.spotify.com/.well-known/openid-configuration)
- [Authorization endpoint](https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint): roughly equivalent to the [OAuth2 authorization endpoint](https://datatracker.ietf.org/doc/html/rfc6749#section-3.1). It authenticates a user with the provider and redirects to the client redirect URI with an authorization code.
- [Token endpoint](https://openid.net/specs/openid-connect-core-1_0.html#TokenEndpoint): this endpoint exchanges the authorization code issued by the **Authorization** endpoint for a long lived access token. Depending on the `grant_type` parameter, this endpoint is also used to refresh a long lived access token.
- (_Optional_) [UserInfo endpoint](https://openid.net/specs/openid-connect-core-1_0.html#UserInfo): an endpoint that returns claims about an authenticated user. The available claims can be described in the **Discovery** endpoint.

### Data structures

This section outlines the Graph API endpoint response data structures.

#### Token endpoint

The **Token** endpoint should return a JSON-encoded result similar to the snippet below (all fields required):

```json
{
   "access_token": "SlAV32hkKG",
   "token_type": "Bearer",
   "refresh_token": "8xLOxBtZp8",
   "expires_in": 3600,
   "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjFlOWdkazcifQ.ewogImlzcyI6ICJodHRwOi8vc2VydmVyLmV4YW1wbGUuY29tIiwKICJzdWIiOiAiMjQ4Mjg5NzYxMDAxIiwKICJhdWQiOiAiczZCaGRSa3F0MyIsCiAibm9uY2UiOiAibi0wUzZfV3pBMk1qIiwKICJleHAiOiAxMzExMjgxOTcwLAogImlhdCI6IDEzMTEyODA5NzAKfQ.ggW8hZ1EuVLuxNuuIJKX_V8a_OMXzR0EHR9R6jgdqrOOF4daGU96Sr_P6qJp6IcmD3HP99Obi1PRs-cwh3LO-p146waJ8IhehcwL7F09JdijmBqkvPeB2T9CJNqeGpe-gccMg4vfKjkM8FcGvnzZUN4_KSP0aAp1tOJ1zZwgjxqGByKHiOtX7TpdQyHE5lcMiKPXfEIQILVq0pc_E2DzL7emopWoaoZTF_m0_N0YzFC6g6EJbOEoRoS     K5hoDalrcvRYLSrQAZZKflyuVCyixEoV9GfNQC3_osjzw2PAithfubEEBLuVVk4XUVrWOLrLl0nx7RkKU8NXNHq-rvKMzqg"
  } 
```

#### UserInfo endpoint

The **UserInfo** response must contain a `sub` value. It can also contain other relevant details for an application, such as name, email, or avatar URL. Here's an example of what this could look like:

```typescript
type UserInfoResponse = {
  /**
  * The id of the authenticated user (the 'subject')
  */
  sub: string
}
```

Example **UserInfo** endpoint response that contains the required `sub` field and some additional (optional) user information:

```json
{
   "sub": "248289761001",
   "name": "Jane Doe",
   "given_name": "Jane",
   "family_name": "Doe",
   "preferred_username": "j.doe",
   "email": "janedoe@example.com",
   "picture": "http://example.com/janedoe/me.jpg"
 }
 ```

### Tokens

We expect **access tokens** to be relatively short-lived (on the order of hours to days, not months or infinite) and to be able to refresh them after they expire using a **one-time refresh token**. Upon refreshing, we expect a new access token and a new refresh token.

### Scope description

To help users understand what authentication is available in the API, Graph API needs a list of scopes with the following structure:

```typescript
type Scope = {
  /**
  * Actual programmatic scope specified during OAuth dance
  */
  scope: string;
  /**
  * Description as to the purpose of the scope / what it grants access to
  */
  description: string;
  /**
  * Human-friendly title of the scope
  */
  title: string;
  /**
  * A category to group multiple scopes under
  */
  category: string;
  /**
  * Whether the scope should be specified by default
  */
  isDefault: boolean;
  /**
  * Whether the scope is required to access the service
  * If `true`, `isDefault` must also be `true`.
  *
  * If this scope is required in order to retrieve the
  * logged-in user information associated with this token
  * (primarily their id in your service), then this field
  * must be `true`.
  */
  isRequired: boolean;
}
```

For example, here is how Spotify outlines the scopes in their own API:

```json
[
  {
    "scope": "ugc-image-upload",
    "display": "ugc-image-upload",
    "description": "Write access to user-provided images.",
    "title": "Upload images to Spotify on your behalf.",
    "category": "Images",
    "isDefault": false,
    "isRequired": false,
  },
  {
    "scope": "user-read-recently-played",
    "display": "user-read-recently-played",
    "description": "Read access to a user’s recently played tracks.",
    "title": "Access your recently played items.",
    "category": "Listening History",
    "isDefault": true,
    "isRequired": false,
  },
  {
    "scope": "user-read-playback-state",
    "display": "user-read-playback-state",
    "description": "Read access to a user’s player state.",
    "title": "Read your currently playing content and Spotify Connect devices information.",
    "category": "Spotify Connect",
    "isDefault": true,
    "isRequired": false,
  }
]
```

## Graph API/Explorer integration

By integrating with the Graph API/Explorer experience, partners can enable developers to interact with their Graph API surface through Netlify Graph, and combine the information from their API with any of the supported services that are already part of the Netlify Graph.

There are two steps to a compatible Graph Explorer integration - **ad-hoc** and **standard**.

## Ad-hoc

To make a Graph Explorer integration available in **ad-hoc** mode (not publicly visible, not integrated in the global index available to other customers), partners need to ensure that their GraphQL endpoints and the data meets the following requirements:

- *Must* implement GraphQL schema introspection.
- Naming requirements:
    - All field names normalized [camelCase](https://techterms.com/definition/camelcase), for example, no `User.first_name`. We can normalize this on our end automatically to `User.firstName`, but it may cause unexpected issues for API providers (and then consequently to consumers).
    - No fields can start with a capital letter.
    - All GraphQL types *must* start with a capital letter and use [PascalCase](https://techterms.com/definition/pascalcase) naming structure.

> **NOTE**
> Keep in mind that in **ad-hoc** mode, the integration is only used for testing and validation, and is not yet ready for adoption by Netlify developers. It's a great way to get started and make sure that partners have the foundational building blocks before moving further.

## Standard

To make the integration ready for production, partners will need to ensure that additional requirements are met:

- [Global Object Identification](https://graphql.org/learn/global-object-identification/) implemented. We need this for the Node Interface [https://dev.to/zth/the-magic-of-the-node-interface-4le1](https://dev.to/zth/the-magic-of-the-node-interface-4le1).
- [Cursor Connection Spec](https://dev.to/zth/connection-based-pagination-in-graphql-2588) implemented. It standardizes pagination, and will enable us to generate automatic functions, e.g. `query.users.pageInfo.hasNextPage ? query.users.fetchMore() : ()` and even automatic nested pagination here in [this example](https://youtu.be/qkkRss6x5ko?t=377), but that requires *both* the GID+Node Interface *and* the Cursor Connection Spec.
- Any top-level fields under query should be resource-oriented, and not action-oriented. For example:
  - Bad: `getUser`, `getUserById`, `getAllUsers`
  - Good: `user(id: $userId)`, `user(email: $email)`, `users(filter: {createdAfter: $date, orderBy: {field: CREATED_AT, direction: DESC})`

The above requires more work on the partner side, but as a result no developer ever writes pagination code (even nested pagination), Netlify infrastructure can poll stale-APIs efficiently without missing items, and it enables building more advanced future capabilities.