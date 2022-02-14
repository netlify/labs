# Get started with Netlify Graph

Netlify Graph can be a powerful addition to your site, but there's a lot consider - especially if you're new to GraphQL. This section will guide you through a basic use case for Netlify Graph.

At a glance, here's what you need to do:
1. [Link a local project directory to a Netlify site](#link-your-local-project-directory-to-your-netlify-site)
2. [Connect to your first API or service](#connect-to-your-first-api-or-service) in the Netlify UI.
3. [Start a local CLI session with Netlify Dev](#start-a-local-cli-session-with-netlify-dev).
4. [Create a GraphQL query with Graph Explorer](#create-a-graphql-query-with-graph-explorer).
5. [Generate a query handler](#generate-a-query-handler).
6. [Use a query handler in your project](#use-a-query-handler-in-your-project).

>**NOTE**: While we specify `query` and `query handler` on this page, you can do all of these things with mutations, subscriptions, and fragments too.

## Link a local project directory to a Netlify site

Netlify Graph syncs your local project with a Netlify site by linking the two through the Netlify CLI.

To link a local project to a Netlify site:
1. Ensure you have the Netlify CLI installed.
  ``` bash
  npm install netlify-cli -g
  ```
2. In your terminal, navigate to the project you want to link, then run `netlify link`.
3. When prompted, select the git remote origin for your project.

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

## Create a GraphQL query with Graph Explorer

To create a new query, click on **New +** and select **Query** - you can then use the **Explorer** button to open the list of available APIs and start composing query details.

Once you compose your query, you can save the changes by pressing **Save Changes**. This will queue up CLI updates. The changes you are making are isolated to your CLI session until you commit the generated content to the repository.

To get the latest persistent query updates in the CLI session and in your local copy, run:

```bash
netlify graph:pull
```

Another added benefit of using the Graph Explorer is that you can generate handlers for your queries. Handlers are auto-generated code that is packaged in Netlify Functions, that you can use from your web application. To generate a handler for your query, click **Generate Handler** in Graph Explorer, and then update your local copy by running:

```bash
netlify graph:pull
```

To use a local development environment for the Graph functionality, that will automate `netlify graph:pull`, you can use:

```bash
netlify dev --graph
```

>**IMPORTANT NOTE**: If you are using Netlify Graph with a Next.js application, make sure to run the commands above inside the application folder instead of the repository root.

To learn more about the `graph` CLI commands, refer to our [Netlify CLI documentation](https://cli.netlify.com/commands/graph/).


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

After generating a query handler and pulling it to your local project, you can access that handler in your project's code. Handlers are called just like other [Netlify Functions](https://docs.netlify.com/functions/build-with-javascript/).

Queries are stored in your local project under `netlify/functions/YOUR_QUERY`, which means that you need to call their endpoints like this: `/.netlify/functions/YOUR_QUERY`. Note the preceding slash and period.

You can test your query handlers locally by running `netlify dev --graph` at your project's root.

> To deploy the functions, commit the changes you've pulled locally to your repository. This should automatically start a deploy of your Netlify site, and at the same time - any of the associated functions.

<!-- QUESTION: Is this still relevant? -->
> With the deployed functions, you will also need to ensure that you are providing an _authentication token_ before being able to receive data from any of the supported services. To provide it, you can update the handler code inside the `netlify/functions` folder in your site repository. Open the file that matches the name of the query that you've built in Graph Explorer, and modify the `accessToken` variable.
> To make the process more streamlined, you can use [API Authentication](authentication.md) to connect to the service of choice through the Netlify dashboard, and use the token from the JavaScript or TypeScript code.

## Learn More

- [Docs home](README.md)
- [Authentication](authentication.md)
- [Graph settings](graph-settings.md)