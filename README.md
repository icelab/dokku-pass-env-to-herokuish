# Dokku “Pass ENV to Herokuish” plugin

A plugin for [Dokku](https://github.com/progrium/dokku) that send the app’s config values as `docker -e` environment variable flags, even for images detected as being Herokuish-based.

Why would you use this? When you deploy `Dockerfile`-based apps to Dokku, then by default you can’t use a `Procfile` and `dokku ps:scale` to run multiple different process containers all with the same app environment. Dokku only offers these features for [Herokuish](https://github.com/gliderlabs/herokuish)-based images, which it uses when you don’t provide a `Dockerfile` or explicitly specify a Heroku buildpack to use.

You can make Dokku _think_ your `Dockerfile`-based images are Herokuish-based by providing these two binaries:

* `/exec`, which should execute any arbitrary command passed to it as arguments
* `/start`, which should run a named process from your app’s `Procfile` (e.g., `/start web`)

Adding these binaries will allow Dokku’s `ps:scale` feature to work with your `Dockerfile`-based images. Excellent! However, it will stop Dokku from passing your app’s config values to the running container as environment variables via `docker -e`, because _true_ Herokuish-based images already have these config values baked in as an additional image layer. This will most likely prevent your app from working at all. Sad face :(

You can make everything good again by installing this plugin: it will ensure these config values still get passed as `docker -e` arguments to your app containers, even if Dokku thinks they’re Herokuish-based.

So in the end, you can write your Dockerfile and eat it too: you’ll have your customised app images running properly on Dokku _and_ working properly with `ps:scale`!
