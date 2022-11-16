# dokku-mailcatcher-2022

Run [MailCatcher](https://mailcatcher.me/) in Dokku.

Tested with **heroku-20** stack.

## Usage

Deploy it next to a target Dokku app you want to catch outgoing emails from.
1. `dokku apps:create mailcatcher`
2. Push this repo to Dokku's **mailcatcher** app, then (optionally) configure DNS and SSL for it.
3. Link the target app for using **mailcatcher** as a SMTP server address: `dokku docker-options:add <target-app> deploy,run --link mailcatcher.web.1:mailcatcher`
4. Set the target app to deliver emails to smtp://mailcatcher:1025 (SMTP server address **mailcatcher** and port **1025**).
