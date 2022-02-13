# Get started with Netlify Graph

Netlify Graph can be a powerful addition to your site, but there's a lot consider - especially if you're new to GraphQL. This section will guide you through a basic use case for Netlify Graph.

At a glance, here's what you need to do:
1. [Link your local project directory to your Netlify site](#link-your-local-project-directory-to-your-netlify-site)
2. [Connect to your first API or service](#connect-to-your-first-api-or-service) in the Netlify UI.
3. [Start a local CLI session with Netlify Dev](#start-a-local-cli-session-with-netlify-dev).
4. [Create a GraphQL query with Graph Explorer](#create-a-graphql-query-with-graph-explorer).
5. [Generate a query handler](#generate-a-query-handler).
6. [Use a query handler in your project](#use-a-query-handler-in-your-project).

## Link your local project directory to your Netlify site


## Connect to your first API or service

For the time being, Netlify Graph can connect to a limited set of APIs and services. Refer to [Supported API Providers](authentication.md#supported-api-providers) to see the full list.

To connect an API or service:
1. In the Netlify UI, select your site, then navigate to the **Graph** tab. 
2. On the **Graph** page, select **Connect to an API or service**.
3. Select one of the available services.
  Note the `Authentication` and `GraphQL` tags. Not all APIs and services offer both features.
<!-- TODO: I made up "Start using Graph for GitHub". Currently, the button text changes depending on whether Authentication or Graph Explorer is selected. However, it doesn't have a state for when both are selected. -->
4. Enable **Authentication** or **GraphQL** by selecting **Enable**, then select **Start using Graph for GitHub**.
  You can set **Authentication** scopes by selecting the **Authentication** dropdown menu.
5. Under **Open sessions**, select a session to start building GraphQL queries for your connected API or service.

## Start a local CLI session with Netlify Dev

To make full use of Netlify Graph, you need Netlify Dev and the Netlify CLI. Make sure you install Netlify CLI and have the latest version.

``` sh
npm install netlify-cli -g
```

To start a new Graph session:
1. In your terminal, navigate to your site's local directory.
2. Run `netlify dev --graph` to start a local development server.


### Interact with Graph through the CLI
<!-- QUESTION: I'm not entirely sure what we want to cover in this section. -->


## Create a GraphQL query with Graph Explorer



## Generate a query handler
With Graph Explorer, you can auto-generated handlers for your queries. These handlers are wrapped in Netlify Functions that you can call in your project. 

To generate a query handler:
1. In the Netlify UI, select your site, then navigate to the **Graph** tab.
2. [Start a new session with Netlify Dev](#start-a-new-session-with-netlify-dev) or select a session under **Open sessions**
3. On your session page, select **Query library** and find the query you want to generate handlers for.
4. On the right-hand side of the page, select **Options**, then **Generate handler**.
5. Optional: Update your local copy by running:
  ```bash
  netlify graph:pull
  ```

## Use a query handler in your project


