## Lighthouse Score Visualizations (Labs feature)

When you install the [Lighthouse Build Plugin](https://app.netlify.com/plugins/@netlify/plugin-lighthouse/install) on your site and enable this experimental feature, you can view the Lighthouse scores for each of your builds on your site's Deploy Details page with a much richer format.

<img width="1400" alt="Deploy view with Lighthouse visualizations" src="https://user-images.githubusercontent.com/79875905/160019039-c3e529de-f389-42bc-a3d4-458c90d59e6a.png">

If you have multiple audits (directories, paths, etc) defined in your build, we will display a roll-up of the average Lighthouse scores for all the current build's audits plus the results for each individual audit.

<img width="1400" alt="Deploy details with multiple audit Lighthouse results" src="https://user-images.githubusercontent.com/79875905/160019057-d29dffab-49f3-4fbf-a1ac-1f314e0cd837.png">

Some items of note:
- The [Lighthouse Build Plugin](https://app.netlify.com/plugins/@netlify/plugin-lighthouse/install) must be installed on your site(s) in order for these score visualizations to be displayed.
- This Labs feature is currently only enabled at the user-level, so it will need to be enabled for each individual team member that wishes to see the Lighthouse scores displayed.

We have a lot planned for this feature and will be adding functionality regularly, but we'd also love to hear your thoughts. Please [share your feedback](https://netlify.qualtrics.com/jfe/form/SV_1NTbTSpvEi0UzWe) about this experimental feature and tell us what you think.