# dokku-mailcatcher-2022

Run [MailCatcher](https://mailcatcher.me/) in Dokku. Deploy it next to a target Dokku app you want to catch outgoing emails from.

Tested with **heroku-20** stack, should work with **heroku-18** one too.

## Simple usage

### Issues upon server reboot
If Dokku's **mailcatcher** app is started *after* the target app, then that target app will fail to start. You may try to play with naming of Dokku apps (AFAIK, upon reboot Dokku starts its apps in alphabetical order), but I didn't check that and went straight to [Advanced usage](#advanced-usage).

### Requirements
Dokku 0.3.17+ if you know how to run **heroku-18**/**heroku-20** stacks on Dokku versions before 0.23.0.

I tested that Dokku 0.4.5 is able to build apps on [gliderlabs/herokuish:v0.5.37-18](https://hub.docker.com/layers/gliderlabs/herokuish/v0.5.37-18/images/sha256-2ed054ebe5c58535d6434cfe483d3f38ca467ad2f682fcedb314baaf7908b0c7?context=explore), which is **heroku-18** released on Aug 7, 2022. But old Dokku versions don't support [customizing the Buildpack stack builder](https://dokku.com/docs/deployment/builders/herokuish-buildpacks/#customizing-the-buildpack-stack-builder) for different apps on the same Dokku server, so your target app must work with **heroku-18**/**heroku-20** stacks too.

### How
1. `dokku apps:create mailcatcher`
2. Push this repo to Dokku's **mailcatcher** app, then (optionally) configure DNS and SSL for it.
3. Link the target app for using **mailcatcher** as a SMTP server address: `dokku docker-options:add <target-app> deploy,run --link mailcatcher.web.1:mailcatcher`
4. Set the target app to deliver emails to **smtp://mailcatcher:1025** (SMTP server address **mailcatcher** and port **1025**).

## Advanced usage

### Requirements
Dokku 0.20.0+, Docker 1.21+.

### How
1. `dokku apps:create mailcatcher`
2. Push this repo to Dokku's **mailcatcher** app, then (optionally) configure DNS and SSL for it.
3. Create a Docker network: `dokku network:create my-app-network`
4. Attach both Dokku apps to the created network: `dokku network:set mailcatcher attach-post-deploy my-app-network; dokku network:set <target-app> attach-post-create my-app-network`
5. Restart both **mailcatcher** and the target app: `dokku ps:restart mailcatcher; dokku ps:restart <target-app>`.
6. Set the target app to deliver emails to **smtp://mailcatcher.web:1025** (SMTP server address **mailcatcher.web** and port **1025**).

## License
Copyright © 2022 [Andrey Bulava](https://github.com/abulava). Released under the MIT License, see [LICENSE][license] for details.

  [license]: https://github.com/abulava/dokku-mailcatcher-2022/blob/master/LICENSE
