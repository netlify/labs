# Scheduled Functions Documentation

Scheduled Functions is a feature of Netlify Functions that enable you to run them on a regular and consistent schedule, much like a cron job. Scheduled functions can do anything that serverless functions do today, though some tasks are better suited to scheduled functions than others.

For example, you may want to:

* Invoke a set of APIs to collate data for a report at the end of every week 
* Backup data from one data store to another at the end of every night 
* Build and/or deploy all your static content every hour instead of for every authored or merged pull request
* Or anything else you can imagine you might want to invoke on a regular basis!

> **NOTE:** Scheduled Functions is a **Beta** feature released through [Netlify Labs](https://www.netlify.com/blog/2021/03/31/test-drive-netlify-beta-features-with-netlify-labs/). Its functionality is subject to change. We strongly recommend not using it in any production and/or critical workflows.

## Getting Started

To get started, you will need to enable the feature in [Netlify Labs](https://app.netlify.com/user/labs). Once you open the Netlify Labs page, click on **Enable** next to the **Scheduled Functions** experimental feature:

![Enable Scheduled Functions in Netlify Labs](../../../media/scheduled-functions/enable-labs-setting.png)

Then, you will have the option to enable scheduled functions on individual sites within your Netlify organization by navigating to the "Functions" tab in the site, and clicking "Enable Scheduled Functions (Labs)":

![Enable Scheduled Functions on your site](../../../media/scheduled-functions/enable-functions-for-site.png)

## Writing a scheduled function

Scheduled functions use the ["cron expression" format used by tools like crontab](https://man7.org/linux/man-pages/man5/crontab.5.html) and are executed according to the UTC timezone.
For example, the cron expression `0 0 * * *` will run a scheduled function every day at midnight UTC. We also support the [extensions](https://man7.org/linux/man-pages/man5/crontab.5.html#EXTENSIONS) in the RFC, except for the `@reboot` and `@annually` specifications.
With extensions, the expression `0 0 * * *` can be written as `@daily`.

There are two ways to specify a cron expression for a scheduled function:

### Cron expression inline in function code

First, make sure you install the `@netlify/functions` npm module to your local project directory:

```sh
npm install @netlify/functions
```

`./netlify/functions/test-scheduled-function.js`:

```javascript
const { schedule } = require('@netlify/functions')

const handler = async function(event, context) {
    console.log("Received event:", event)

    return {
        statusCode: 200,
    };
};

module.exports.handler = schedule("@hourly", handler);
```

### Cron expression in `netlify.toml`

If you prefer to keep your cron expressions in one file and separate from your function code, you can specify them in the `netlify.toml` configuration at the root of your repository.

`./netlify/functions/test-function.js`:

```javascript
const handler = async function(event, context) {
    console.log("Received event:", event)

    return {
        statusCode: 200,
    };
};

module.exports.handler = handler;
```

`./netlify.toml`:

```toml
[functions."test-function"]
schedule = "@hourly"
```

> Note: if you're using a function language other than JavaScript or TypeScript, you must specify your cron expression in `netlify.toml`

## Developing and debugging scheduled functions

Today, scheduled functions only run on published deploys. (E.g. deploy previews will not execute scheduled functions.) And similar to [event-triggered functions](https://docs.netlify.com/functions/trigger-on-events/), scheduled functions that are published as part of a deploy cannot be invoked directly with a URL.

Therefore, the best way to test a scheduled function is to use [Netlify Dev](https://github.com/netlify/cli/blob/main/docs/netlify-dev.md) to [serve and debug your scheduled functions locally](https://cli.netlify.com/functions-dev/), and then to use the [`netlify functions:invoke myfunction`](https://cli.netlify.com/commands/functions#functionsinvoke) command to invoke it. You can also invoke functions locally on the URL path, but those invocations are purely for interactive debugging, and will wrap the function response with a note that this URL invocation is not possible in production.

> Note: Netlify Dev will not execute the scheduled function on any kind of schedule. This is simply to debug the invocation of the function code.

Alternatively, you can create a new test site to experiment with scheduled functions as part of published deploys.

If you've done everything correctly, your deployed function should appear in the Functions pane of the Netlify app with a "Scheduled" badge. The function will show a next execution, converted to the user's timezone.

![Function list in the "Functions" pane](../../../media/scheduled-functions/functions-list.png)

Clicking on the scheduled function, you can see the function's schedule and tail the function's logs.

![Example of a scheduled function in the Netlify app](../../../media/scheduled-functions/function-in-app.png)

## Supported cron extensions:

- `@yearly`: once a year, on January 1st 00:00 (`0 0 1 1 *`)
- `@monthly`: every month, on the first day of the month, at 00:00 (`0 0 1 * *`)
- `@weekly`: every Sunday, 00:00 (`0 0 * * 0`)
- `@daily`: once a day, at 00:00 (`0 0 * * *`)
- `@hourly`: every hour, at minute 0 (`0 * * * *`)
