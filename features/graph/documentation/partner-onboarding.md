# Partner onboarding to Netlify Graph

This document outlines the requirements for partners to integrate with [Netlify Graph](https://www.netlify.com/blog/announcing-netlify-graph-a-faster-way-for-teams-to-develop-web-apps-with-apis). By integrating with the Graph experience, partners can expose two key components to their customers:

1. API Authentication
2. Graph API/Explorer integration

Both can be integrated with separately, as they are independent of each other. However, they do work best in tandem, so where possible, we recommend integrating with both.

## API Authentication


### Endpoints

We require API providers to implement the [Open ID Connect (OIDC)](https://openid.net/connect/) standard. OIDC can be thought of an extension to OAuth2, and it’s generally feasible to build OIDC features on top of a working OAuth2 setup.

In practical terms, this roughly means implementing the following endpoints:

- [Discovery endpoint](https://openid.net/specs/openid-connect-discovery-1_0.html): a JSON endpoint describing metadata about the where to find the remaining endpoints described in this section.
    - Here’s an example of Google’s: [https://accounts.google.com/.well-known/openid-configuration](https://accounts.google.com/.well-known/openid-configuration) and Spotify’s [https://accounts.spotify.com/.well-known/openid-configuration](https://accounts.spotify.com/.well-known/openid-configuration)
- [Authorization endpoint](https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint): roughly equivalent to [OAuth2’s authorization endpoint](https://datatracker.ietf.org/doc/html/rfc6749#section-3.1), authenticates a user with the provider and redirects to the client redirect URI with an authorization code.
- [Token endpoint](https://openid.net/specs/openid-connect-core-1_0.html#TokenEndpoint): this endpoint exchanges the authorization code issued by the Authorization endpoint for a long lived access token. Depending on the `grant_type` parameter, this endpoint is also used to refresh a long lived access token.
- (_Optional_) [UserInfo endpoint](https://openid.net/specs/openid-connect-core-1_0.html#UserInfo): an endpoint that returns claims about an authenticated user. The available claims can be described in the Discovery endpoint.

### Data structures

#### Token Endpoint

```json
{
   "access_token": "SlAV32hkKG",
   "token_type": "Bearer",
   "refresh_token": "8xLOxBtZp8",
   "expires_in": 3600,
   "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjFlOWdkazcifQ.ewogImlzc
     yI6ICJodHRwOi8vc2VydmVyLmV4YW1wbGUuY29tIiwKICJzdWIiOiAiMjQ4Mjg5
     NzYxMDAxIiwKICJhdWQiOiAiczZCaGRSa3F0MyIsCiAibm9uY2UiOiAibi0wUzZ
     fV3pBMk1qIiwKICJleHAiOiAxMzExMjgxOTcwLAogImlhdCI6IDEzMTEyODA5Nz
     AKfQ.ggW8hZ1EuVLuxNuuIJKX_V8a_OMXzR0EHR9R6jgdqrOOF4daGU96Sr_P6q
     Jp6IcmD3HP99Obi1PRs-cwh3LO-p146waJ8IhehcwL7F09JdijmBqkvPeB2T9CJ
     NqeGpe-gccMg4vfKjkM8FcGvnzZUN4_KSP0aAp1tOJ1zZwgjxqGByKHiOtX7Tpd
     QyHE5lcMiKPXfEIQILVq0pc_E2DzL7emopWoaoZTF_m0_N0YzFC6g6EJbOEoRoS
     K5hoDalrcvRYLSrQAZZKflyuVCyixEoV9GfNQC3_osjzw2PAithfubEEBLuVVk4
     XUVrWOLrLl0nx7RkKU8NXNHq-rvKMzqg"
  } 
```

#### UserInfo endpoint

The `userInfo` response should, at a minimum, contain `sub`. It can also contain other relevant details for an application, such as name, email, avatar url, etc.:

```typescript
type UserInfoResponse = {
  /**
  * The id of the authenticated user (the 'subject')
  */
  sub: string
}
```

```json
// Example userInfo that contains the required `sub` field as well as some additional (optional) user information
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

### Tokens (refresh, etc.)

We expect `access token`s to be relatively short-lived (on the order of hours to days, not months or infinite), and to be able to refresh them after they expire using a one-time `refresh token`. Upon refreshing, we expect a new `access token` and a new `refresh token`.

### Scope description

To show the user a friendly interface to understand what authentication is available in the API, we need a list of scopes with the following structure:

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
  isDefault: string;
  /**
  * Whether the scope is required to access the service
  * If true, `isDefault` must be true.
  *
  * If this scope is required in order to retrieve the
  * logged-in user information associated with this token
  * (primarily their id in your service), then this field
  * must be true.
  */
  isRequired: string;
}
```

For example, here is how Spotify outlines the scopes in their own API:

```json
[
  {
    scope: "ugc-image-upload",
    display: "ugc-image-upload",
    description: "Write access to user-provided images.",
    title: Some("Upload images to Spotify on your behalf."),
    category: Some("Images"),
    isDefault: false,
    isRequired: false,
  },
  {
    scope: "user-read-recently-played",
    display: "user-read-recently-played",
    description: "Read access to a user’s recently played tracks.",
    title: Some("Access your recently played items."),
    category: Some("Listening History"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-read-playback-state",
    display: "user-read-playback-state",
    description: "Read access to a user’s player state.",
    title:
      Some(
        "Read your currently playing content and Spotify Connect devices information.",
      ),
    category: Some("Spotify Connect"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-top-read",
    display: "user-top-read",
    description: "Read access to a user's top artists and tracks.",
    title: Some("Read your top artists and content."),
    category: Some("Listening History"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "app-remote-control",
    display: "app-remote-control",
    description: "Remote control playback of Spotify. This scope is currently available to Spotify iOS and Android SDKs.",
    title: Some("Communicate with the Spotify app on your device."),
    category: Some("Playback"),
    isDefault: false,
    isRequired: false,
  },
  {
    scope: "playlist-modify-public",
    display: "playlist-modify-public",
    description: "Write access to a user's public playlists.",
    title: Some("Manage your public playlists."),
    category: Some("Playlists"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-modify-playback-state",
    display: "user-modify-playback-state",
    description: "Write access to a user’s playback state",
    title:
      Some(
        "Control playback on your Spotify clients and Spotify Connect devices.",
      ),
    category: Some("Spotify Connect"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "playlist-modify-private",
    display: "playlist-modify-private",
    description: "Write access to a user's private playlists.",
    title: Some("Manage your private playlists."),
    category: Some("Playlists"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-follow-modify",
    display: "user-follow-modify",
    description: "Write/delete access to the list of artists and other users that the user follows.",
    title: Some("Manage who you are following."),
    category: Some("Follow"),
    isDefault: false,
    isRequired: false,
  },
  {
    scope: "user-read-currently-playing",
    display: "user-read-currently-playing",
    description: "Read access to a user’s currently playing content.",
    title: Some("Read your currently playing content."),
    category: Some("Spotify Connect"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-follow-read",
    display: "user-follow-read",
    description: "Read access to the list of artists and other users that the user follows.",
    title: Some("Access your followers and who you are following."),
    category: Some("Follow"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-library-modify",
    display: "user-library-modify",
    description: "Write/delete access to a user's \"Your Music\" library.",
    title: Some("Manage your saved content."),
    category: Some("Library"),
    isDefault: false,
    isRequired: false,
  },
  {
    scope: "user-read-playback-position",
    display: "user-read-playback-position",
    description: "Read access to a user’s playback position in a content.",
    title: Some("Read your position in content you have played."),
    category: Some("Listening History"),
    isDefault: false,
    isRequired: false,
  },
  {
    scope: "playlist-read-private",
    display: "playlist-read-private",
    description: "Read access to user's private playlists.",
    title: Some("Access your private playlists."),
    category: Some("Playlists"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-read-email",
    display: "user-read-email",
    description: "Read access to user’s email address.",
    title: Some("Get your real email address."),
    category: Some("Users"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-read-private",
    display: "user-read-private",
    description: "Read access to user’s subscription details (type of user account).",
    title: Some("Access your subscription details."),
    category: Some("Users"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "user-library-read",
    display: "user-library-read",
    description: "Read access to a user's library.",
    title: Some("Access your saved content."),
    category: Some("Library"),
    isDefault: true,
    isRequired: false,
  },
  {
    scope: "playlist-read-collaborative",
    display: "playlist-read-collaborative",
    description: "Include collaborative playlists when requesting a user's playlists.",
    title: Some("Access your collaborative playlists."),
    category: Some("Playlists"),
    isDefault: false,
    isRequired: false,
  },
  {
    scope: "streaming",
    display: "streaming",
    description: "Control playback of a Spotify track. This scope is currently available to the Web Playback SDK. The user must have a Spotify Premium account.",
    title: Some("Play content and control playback on your other devices."),
    category: Some("Playback"),
    isDefault: false,
    isRequired: false,
  },
]
```

## Graph API/Explorer integration