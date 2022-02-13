# Netlify Graph

> **NOTE:** Netlify Graph is a **Beta** feature released through [Netlify Labs](https://www.netlify.com/blog/2021/03/31/test-drive-netlify-beta-features-with-netlify-labs/). Its functionality is subject to change. We strongly recommend not using it in production or any critical workflows.

## Table of Contents
<!-- TODO: Update TOC after content has been re-organized -->
- [Overview](#overview)
- [Components](#components)
- [Enabling the feature](#enabling-the-feature)
- [Feedback](#feedback)

## Overview

Netlify Graph enables developers to seamlessly integrate third-party APIs and services into their web applications without writing API-specific code. Instead of connecting different APIs and SDKs with brittle code, you can now abstract all that behind a convenient GraphQL interface.

![Introduction to Netlify Graph.](../../../media/graph/graph-intro.png)

Essentially, Netlify handles the messy integration work so you can focus on solving other problems.

Netlify Graph and its component features are available for all sites in your Netlify team by default.
### Components

Netlify Graph has three core components. You can learn more about each by reading their documentation pages:

- [API Management](management.md)
- [Graph Explorer](graph-explorer.md)
- [Authentication](authentication.md)

## Requirements

Netlify Graph uses the Netlify CLI to communicate between your local project and your Netlify site. Before you do anything else, you should install the latest version of the Netlify CLI.

``` bash
npm install netlify-cli -g
```

## Get started with Netlify Graph

<!-- TODO: Expand on this -->
- something about these, with links to the getting started content:
  1. [Link your local project directory to your Netlify site](authentication.md#link-your-local-project-directory-to-your-netlify-site)
  2. [Connect to your first API or service](authentication.md#connect-to-your-first-api-or-service) in the Netlify UI.
  3. [Start a local CLI session with Netlify Dev](authentication.md#start-a-local-cli-session-with-netlify-dev).
  4. [Create a GraphQL query with Graph Explorer](authentication.md#create-a-graphql-query-with-graph-explorer).
  5. [Generate a query handler](authentication.md#generate-a-query-handler).
  6. [Use a query handler in your project](authentication.md#use-a-query-handler-in-your-project).

## Queries, mutations, and subscriptions
<!-- QUESTION: Are we going to address fragments? -->
<!-- QUESTION: Is my assumption that these are standard GraphQL correct? -->
You use standard GraphQL queries, mutations, and subscriptions to interact with your Netlify Graph data.

- **[Queries](https://graphql.org/learn/queries/).** Are how you get data.
- **[Mutations](https://graphql.org/learn/queries/#mutations).** Are how you modify data. 
- **[Subscriptions](https://www.onegraph.com/docs/subscriptions.html).** Let you receive data in response to an event.

<!-- TODO: Possibly add examples, though I think linking to external sources is probably the best way to go. If we want to expand this section, we could have subsections for each type. -->

## Access secrets with Netlify Graph

You can access secrets for all of the APIs and services you connect to Netlify Graph. Check out the [Authentication docs](/authentication.md#basic-secret-handling) for details.

## Netlify Graph and Next.js

If you are building a Next.js application, you can automatically generate Netlify Graph bindings by running the Netlify Graph commands inside your application folder.

<!-- QUESTION: perhaps this is my unfamiliarity with Next.js, but what's the difference between the application root and the repository root? They're not the same? -->
> **NOTE**: Make sure to run the Netlify Graph commands in the root folder where your Next.js application is located. In the current CLI release, running in the root of the repository and not the application will result in an error.

## Disable Netlify Graph

Not interested in Netlify Graph? You can opt out by selecting your avatar in the top-right, then selecting Netlify Labs. Under **Experimental features**, find Netlify Graph and select **Disable**.

## Feedback

If you have any feedback on the feature, make sure to [fill out our survey](https://ntl.fyi/apiauthsurvey).