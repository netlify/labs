# Netlify Graph

> **NOTE:** Netlify Graph is a **Beta** feature released through [Netlify Labs](https://www.netlify.com/blog/2021/03/31/test-drive-netlify-beta-features-with-netlify-labs/). Its functionality is subject to change. We strongly recommend not using it in any production and/or critical workflows.

## Table of Contents

- [Overview](#overview)
- [Components](#components)
- [Enabling the feature](#enabling-the-feature)
- [Feedback](#feedback)

## Overview

Netlify Graph enables developers to seamlessly integrate third-party APIs and services into their web applications without writing API-specific code. Instead of connecting different APIs and SDKs with brittle code, you can now abstract all that behind a convenient GraphQL interface.

Essentially, Netlify handles the messy integration work so you can focus on solving other problems.

### Components

Netlify Graph has three core components. You can learn more about each by reading their documentation pages:

- [API Management](api-management.md)
- [API Explorer](api-explorer.md)
- [API Authentication](api-authentication.md)

## Connect to your first API or service
<!-- TODO: Add screenshot of zero state `/sites/netlify-graph-ui-text/graph`. Need to wait until UI is more stable. -->

For the time being, Netlify Graph can connect to a limited set of APIs and services. Refer to [link]() to see the full list.

To connect an API or service:
1. If you haven't yet, select your site, then navigate to the **Graph** tab. 
2. On the **Graph** page, select **Connect to an API or service**.
3. Select one of the available services.
  Note the `Authentication` and `GraphQL` tags. Not all APIs and services offer both features.
<!-- TODO: I made up Start using Graph for GitHub". Currently, the button text changes depending on whether Authentication or Graph Explorer is selected. However, it doesn't have a state for when both are selected. -->
4. Enable **Authentication** or **GraphQL** by selecting **Enable**, then select **Start using Graph for GitHub**.
  You can set **Authentication** scopes by selecting the **Authentication** dropdown menu.
<!-- QUESTION: What does this look like if there isn't an open session? -->
5. Select an open session to start building GraphQL queries for your connected API or service.

## Start a new session with Netlify Dev
To make full use of Netlify Graph, you need Netlify Dev and the Netlify CLI. Make sure you install Netlify CLI and have the latest version.

``` sh
npm install netlify-cli -g
```

To start a new Graph session:
1. In your terminal navigate to your site's local directory.
2. Run `netlify dev --graph` to start a local development server.

### Interact with Graph through the CLI


## Queries, mutations, and subscriptions
<!-- QUESTION: Are we going to address fragments? -->
<!-- QUESTION: Is my assumption that these are standard GraphQL correct? -->
You use standard GraphQL queries, mutations, and subscriptions to interact with your Netlify Graph data.

- **[Queries](https://graphql.org/learn/queries/).** Are how you get data.
- **[Mutations](https://graphql.org/learn/queries/#mutations).** Are how you modify data. 
- **[Subscriptions](https://www.onegraph.com/docs/subscriptions.html).** Let you receive data in response to an event.

<!-- TODO: Possibly add examples, though I think linking to external sources is probably the best way to go. If we want to expand this section, we could have subsections for each type. -->

## Accessing secrets


## Code generation in Graph


## Netlify Graph and Next.js


## Disable Netlify Graph

Not interested in Netlify Graph? You can opt out by selecting your avatar in the top-right, then selecting Netlify Labs. Under **Experimental features**, find Netlify Graph and select **Disable**.

## Feedback

If you have any feedback on the feature, make sure to [fill out our survey](https://ntl.fyi/apiauthsurvey).