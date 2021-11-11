# API Authentication Documentation

> **NOTE:** API Authentication is a **Beta** feature released through [Netlify Labs](https://www.netlify.com/blog/2021/03/31/test-drive-netlify-beta-features-with-netlify-labs/). Its functionality is subject to change. We strongly recommend not using it in any production and/or critical workflows.

API Authentication is a new feature in Netlify that helps you simplify API authentication and token management. Once you connect to one of the API providers available in the Beta release, you can use the authentication tokens from those in your builds and [Netlify Functions](https://www.netlify.com/products/functions/). These tokens are securely stored and available as environment variables.

The API Authentication feature handles token refresh and scope management on your behalf, so you will not need to do anything extra to ensure that those work over time.

## Getting Started

To get started, you will need to enable the feature in [Netlify Labs](https://app.netlify.com/user/labs). Once you open the Netlify Labs page, click on **Enable** next to the **API Authentication** experimental feature.

_**⚠️ TODO** Image of feature in Netlify Labs_

This will automatically make the feature available for all sites you have in your Netlify account, under **API Authentication** in site settings.

_**⚠️ TODO** Image of feature in a site_

From this tab, you can enable the feature for the selected site.

> **IMPORTANT:** Enabling API Authentication for a site will trigger a re-deploy of the site.

Authentication tokens are specific for each individual site, so if you enable one of the API providers for one site, the token will not be re-usable on other sites. You need to authenticate with the same provider again if you would like to use it on a different site.
