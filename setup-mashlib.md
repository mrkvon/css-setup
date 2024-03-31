# Setup mashlib as default app

Everything is running, but you also want a default web app, so users can see and manage their data from browser.

There are 2 documented choices in [CommunitySolidServer/Recipes](https://github.com/CommunitySolidServer/Recipes/) &mdash; mashlib and Penny.

We'll go for mashlib, but Penny or other app will work analogously.

There's [CSS + mashlib config](https://github.com/CommunitySolidServer/Recipes/blob/main/mashlib/config-mashlib.json) in the Recipes.

## Clone and build mashlib

```sh
# run as [user]
cd ~/www/[projectname]
git clone https://github.com/SolidOS/mashlib.git
cd mashlib
npm ci
npm run build
```

If you have limited space on VPS, you can also build the project locally and put the contents of `dist` to the server with `sftp`.

You can also install it from npm:

```sh
# run as [user]
mkdir -p ~/www/[projectname]/mashlib
cd ~/www/[projectname]/mashlib
# install mashlib
npm init -y
nvm exec 18 npm i mashlib
# make symbolic link to the same place as the method above
ln -s $(pwd)/node_modules/mashlib/dist .
```

Now, there should be the following folder structure for mashlib

```
www/[projectname]/
├── mashlib/
│   └── dist/
│       ├── databrowser.html
│       ├── mash.css
│       ├── mashlib.min.js
│       └── mashlib.min.js.map
data, config, and the rest
```

Add the [following section](https://github.com/CommunitySolidServer/Recipes/blob/922dd8005f060568c1d8f349de46c1499ca26e20/mashlib/config-mashlib.json#L44-L80) to your `~/www/[projectname]/config.json`:

```json
  "@graph": [
    {
      // ...
      // other stuff here
      // ...
    },
    // here starts the relevant config for mashlib
    {
      "comment": "Serve Databrowser as default representation",
      "@id": "urn:solid-server:default:DefaultUiConverter",
      "@type": "ConstantConverter",
      "contentType": "text/html",
      "filePath": "./mashlib/dist/databrowser.html",
      "options_container": true,
      "options_document": true,
      "options_minQuality": 1,
      "options_disabledMediaRanges": [
        "image/*",
        "application/pdf"
      ]
    },
    {
      "comment": "Serve Mashlib static files.",
      "@id": "urn:solid-server:default:StaticAssetHandler",
      "@type": "StaticAssetHandler",
      "assets": [
        {
          "@type": "StaticAssetEntry",
          "relativeUrl": "/mash.css",
          "filePath": "./mashlib/dist/mash.css"
        },
        {
          "@type": "StaticAssetEntry",
          "relativeUrl": "/mashlib.min.js",
          "filePath": "./mashlib/dist/mashlib.min.js"
        },
        {
          "@type": "StaticAssetEntry",
          "relativeUrl": "/mashlib.min.js.map",
          "filePath": "./mashlib/dist/mashlib.min.js.map"
        }
      ]
    }
  ]
```

Also replace `"css:config/app/init/static-root.json"` with `"css:config/app/init/default.json"`.

Also remove `"css:config/util/index/default.json"`.

The final config for multiple users may [look like this](config-mashlib.json).

Restart the server and test it:

```sh
# run as [user]
# find id of css process
pm2 list
# restart the process
pm2 restart [process_id-of-css]
```

[Next: Backup the data](backup.md)
