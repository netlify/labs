# Netlify Graph - API Explorer

The API Explorer experience in Netlify Graph allows you to construct queries against supported APIs and generate supporting code directly from the Netlify user interface.

To start experimenting with API Explorer, the easiest way to do it is by using one of our samples.

1. Use the `Deploy to Netlify` button on our test repository [https://github.com/sgrove/secrets-test](https://github.com/sgrove/secrets-test) to copy its contents into your own GitHub/GitLab account.
2. Check out your variant of the repository locally with the help of `git clone <YOUR_REPO_URL>`.
3. Navigate to the cloned folder in the Terminal: `cd <FOLDER_WHERE_YOU_CLONED_REPO>`.
4. Install `netlify-cli` by running `npm install netlify-cli` on your local computer. You must have [Node.js tooling](https://nodejs.org/en/download/) installed. This is the pre-release version of the Netlify CLI - you **cannot use the production version of the Netlify CLI** today to test the Netlify Graph functionality.
5. Run `netlify link` to run the Netlify CLI from the location where you installed the pre-release version of the Netlify CLI. This will prompt you to link your Netlify account to the current CLI session. Because you’ve already created a site based on the cloned repository when you used the **Deploy to Netlify** button, use the “*Use current git remote origin ([https://github.com/](https://github.com/localden/netlify-graph-test)<YOUR_REPO_URL>)***”** option in the CLI. If the command is successful, you will see the admin and site URLs shown in the terminal.

![Linking Netlify Graph from the PowerShell console on Windows](../../../media/graph/terminal-graph-status.gif)

You are now ready to start experimenting with the Netlify Graph functionality. Run `netlify graph:edit` (you’ll need to log into the deploy preview site, so you may need to run this command twice). This will take you to the Netlify Graph API Explorer UI, where you will be able to compose new queries and mutations.

You are now ready to try different APIs. To start, you will need to enable the APIs that you want to work with in the [API Management](api-management.md) interface. Once you enable the desired APIs, you can use the Netlify CLI to start creating new Netlify Graph API interactions.

To get started, run the following command in the CLI, from within your repository folder:

```bash
netlify graph:edit
```

![Kickstarting Netlify Graph editing from the CLI](../../../media/graph/edit-graph.gif)

Running this command will automatically open the Netlify Graph API Explorer in your browser. To create a new query, click on **New +** and select **Query** - you can then use the **Explorer** button to open the list of available APIs and start composing query details.

![Using the API Explorer in Netlify Graph](../../../media/graph/graph-explorer.gif)

Once you compose your query, you can save the changes by pressing **Save Changes**. This will queue up CLI updates. The changes you are making are isolated to your CLI session until you commit the generated content to the repository.

To get the latest persistent query updates in the CLI session and in your local copy, run:

```bash
netlify graph:pull
```

Another added benefit of using the API Explorer is that you can generate handlers for your queries. Handlers are auto-generated code that is packaged in Netlify Functions, that you can use from your web application. To generate a handler for your query, click **Generate Handler** in API Explorer, and then update your local copy by running:

```bash
netlify graph:pull
```

To use a local development environment for the Graph functionality, that will automate `netlify graph:pull`, you can use:

```bash
netlify dev --graph
```

>**IMPORTANT NOTE**: If you are using Netlify Graph with a Next.js application, make sure to run the commands above inside the application folder instead of the repository root.

To learn more about the `graph` CLI commands, refer to our [Netlify CLI documentation](https://cli.netlify.com/commands/graph/).

## Bindings for Next.js

If you are building a Next.js application, you can automatically generate Netlify Graph bindings by running the Netlify Graph commands inside your application folder.

> **NOTE**: Make sure to run the Netlify Graph commands in the root folder where your Next.js application is located. In the current CLI release, running in the root of the repository and not the application will result in an error.

## Usage

With generated handlers, you will now have fully-functioning Netlify Functions that can be used to query the relevant information based on query definitions you've created in API Explorer. To deploy the functions, commit the changes you've pulled locally to your repository. This should automatically start a deploy of your Netlify site, and at the same time - any of the associated functions.

With the deployed functions, you will also need to ensure that you are providing an _authentication token_ before being able to receive data from any of the supported services. To provide it, you can update the handler code inside the `netlify/functions` folder in your site repository. Open the file that matches the name of the query that you've built in API Explorer, and modify the `accessToken` variable.

To make the process more streamlined, you can use the [API Authentication functionality](api-authentication.md) to connect to the service of choice through the Netlify dashboard, and use the token from the JavaScript or TypeScript code.

## Feedback

If you have any feedback on the feature, make sure to [fill out our survey](https://ntl.fyi/apiauthsurvey).

## Learn More

- [API Management](api-management.md)
- [API Authentication](api-authentication.md)
