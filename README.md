# Cloud Foundry Static Starter

This is a reference for configuring a client-side only app that will be served from Cloud Foundry. Copy the other files in this repo into your own.

_**IMPORTANT**_  
This setup _assumes_ that the client-side app will be compiled into a directory named `build` located in the projects top level directory.

#### Staticfile
Configuration specific to a **static** Cloud Foundry app

<details>
  <summary>`root: build`</summary>
  Tells Cloud Foundry that all of your publicly available assets, including `index.html`, will be located in the `/build` directory. If you wish to rename that directory, change this setting along with the directory name in the `.cfignore` file. If you wish to serve static files from the project directory you can remove this setting and carefully reconfigure the `.cfignore` file.
</details>

<details>
  <summary>`pushstate: enabled`</summary>
  Keeps browser-visible URLs clean for client-side JavaScript apps that serve multiple routes. For example, pushstate routing allows a single JavaScript file route to multiple anchor-tagged URLs that look like `/some/path1` instead of `/some#path1`.
</details>

#### manifest.yml
Cloud Foundry app configuration

<details>
  <summary>`name: my-app-name`</summary>
  The name Cloud Foundry name of your app. Change this before pushing your app to CF.
</details>

<details>
  <summary>`memory: 64M`</summary>
  The memory used by Cloud Foundry to serve your app. We shouldn't every need to increase this — serving static files using only NGINX doesn't take much memory.
</details>

<details>
  <summary>`buildpack: staticfile_buildpack`</summary>
  Buildpacks tell Cloud Foundry what tech needs to be installed to run your app and default instructions on how to run it. Here we tell CF that we only need what is required for static files — NGINX.
</details>

<details>
  <summary>`instances: 1`</summary>
  Tells Cloud Foundry to only create one server instance for this app. During development, this should be kept at `1` instance.
</details>

#### .cfignore
Rules for not uploading files to Cloud Foundry

Not uploading files that are useless in production makes it easier for CloudFoundry to stop and start instances of the app. A quick load time keeps our website resilient and minimizes costs.

Follows the same patterns as a `.gitignore` file.

<details>
  <summary>`/*`</summary>
  This entry blacklists all directories and files at the top level only. Doing this only one level deeps makes it easier to whitelist a directory without have to recursively whitelist all of its contents.
</details>

<details>
  <summary>`!/.cfignore`</summary>
  Whitelists the `.cfignore` file.
</details>

<details>
  <summary>`!/manifest.yml`</summary>
  Whitelists the `manifest.yml` file.
</details>

<details>
  <summary>`!/Staticfile`</summary>
  Whitelists the `Staticfile` file.
</details>

<details>
  <summary>`!/build`</summary>
  Whitelist the `build` directory. Because we didn't blacklist the files in every folder recursively, a `cf push` command will include all of this directories contents as well. Change the name of this directory if compiling production assets to a different directory.
</details>
