# Export your Slack workspace as static HTML

Alright, so you want to export all your messages on Slack. You want them in a format that you
can still enjoy in 20 years. This tool will help you do that.

 * **Completely static**: The generated files are pure HTML and will still work in 50 years.
 * **Everything you care about**: This tool downloads messages, files, and avatars.
 * **All conversations**: We'll fetch public channels, private channels, DMs, and multi-person DMs.
 * **No cloud, free**: Do all of this for free, without giving anyone your information.

<img width="1151" alt="Screen Shot 2021-09-09 at 6 43 55 PM" src="https://user-images.githubusercontent.com/1426799/132776566-0f75a1b4-4b9a-4b53-8a39-e44e8a747a68.png">

## Using it

1. Do you already have a user token for your workspace? If not, read on below on how to get a token.
2. Make sure you have [`node` and `npm`](https://nodejs.org/en/) installed, ideally something newer than Node v14.
3. Run `slack-archive`, which will interactively guide you through the options.

```sh
npx slack-archive
```

## Getting a token

In order to download messages from private channels and direct messages, we will need a "user
token". Slack uses the token to identify what permissions it'll give this app. We used to be able
to just copy a token out of your Slack app, but now, we'll need to create a custom app and jump
through a few hoops.

This will be mostly painless, I promise.

### 1) Make a custom app

Head over to https://api.slack.com/apps and `Create New App`. Select `From scratch`.
Give it a name and choose the workspace you'd like to export.

Then, from the `Features` menu on the left, select `OAuth & Permission`. 

As a redirect URL, enter something random that doesn't actually exist. For instace:

```
https://notarealurl.com/
```

Then, add the following `Scopes`:

 * channels:history
 * channels:read
 * files:read
 * groups:history
 * im:history
 * mpim:history
 * remote_files:read

Finally, head back to `Basic Information` and make a note of your app's `client
id` and `client secret`. We'll need both later.

### 2) Authorize

Make sure you have your Slack workspace `URL` (aka team name) and your app's `client id`.
Then, in a browser, open this URL - replacing `{your-team-name}` and `{your-client-id}`
with your values.

```
https://{your-team-name}.slack.com/oauth/authorize?client_id={your-client-id}&scope=client
```

Confirm everything until Slack sends you to the mentioned non-existent URL. Look at your
browser's address bar - it should contain an URL that looks like this:

```
https://notarealurl.com/?code={code}&state=
```

Copy everything between `?code=` and `&state`. This is your `code`. We'll need it in the
next step.

Next, we'll exchange your code for a token. To do so, we'll also need your `client secret` 
from the first step when we created your app. In a browser, open this URL - replacing 
`{your-team-name}`, `{your-client-id}`, `{your-code}` and `{your-client-secret}` with 
your values.

```
https://{your-team-name}.slack.com/api/oauth.access?client_id={your-client-id}&client_secret={your-client-secret}&code={your-code}"
```

Your browser should now be returning some JSON including a token. Make a note of it - that's what we'll use.
