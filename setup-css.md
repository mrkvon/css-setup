# Setup the Community Solid Server

We can already run CSS and access it securely over the internet.

The default CSS config isn't suitable for production. For example, it stores data in memory only.

## Custom config

Copy and edit config from [available configurations](https://github.com/CommunitySolidServer/CommunitySolidServer/tree/main/config). There are also [Recipes](https://github.com/CommunitySolidServer/Recipes/).

For multi-user configuration, we can start from [file.json](https://github.com/CommunitySolidServer/CommunitySolidServer/blob/main/config/file.json).

```sh
# run as [user]
# go to project folder
cd ~/www/[projectname]
# fetch the config
wget https://raw.githubusercontent.com/CommunitySolidServer/CommunitySolidServer/main/config/file.json
mv file.json config.json
```

For single-user configuration, start from [file-root-pod.json](https://github.com/CommunitySolidServer/CommunitySolidServer/blob/main/config/file-root-pod.json):

```sh
# run as [user]
# go to project folder
cd ~/www/[projectname]
# fetch the config
wget https://raw.githubusercontent.com/CommunitySolidServer/CommunitySolidServer/main/config/file-root-pod.json
mv file-root-pod.json config.json
```

Open it and change email and password.

If you already know your preferred configuration, put your config (for the appropriate CSS version) at `~/www/[projectname]/config.json`.

In order to understand the configuration options better, you can compare configs with `diff` and see the relevant changes.

```sh
diff default.json file.json
```

## Data

If you already have existing data, make sure they're backed up, and copy them to `www/[projectname]/data`:

```sh
# run as [user]
cp -r /path/to/data ~/www/[projectname]/data
```

Otherwise initialize empty folder:

```sh
mkdir ~/www/[projectname]/data
```

## Run the server

Now that we have the config ready, it's time to run the server.

We run the server with the following options:

- `-f` - path to folder with data
- `-c` - path to custom config
- `-p` - port number
- `-b` - base url for the server

See more runtime [configuration options]().

### Use [npx](install-css.md#option-1---npx) or [npm](install-css.md#option-2---npm)

```sh
# run as [user]
# go to project folder
cd ~/www/[projectname]
# start the server (npx)
nvm exec 18 npx -y @solid/community-server@7 -f ./data/ -c ./config.json -p [port] -b https://[your.domain]
# or (npm)
nvm exec 18 community-solid-server -f ./data/ -c ./config.json -p [port] -b https://[your.domain]

```

### Use [git repository](install-css.md#option-3---github-repository)

```sh
# run as [user]
# go to cloned and installed CSS repository
cd ~/www/[projectname]/CommunitySolidServer
# start the server
nvm exec 18 npm start -- -f ../data/ -c ../config.json -p [port] -b https://[your.domain]
```

---

The Solid server should be accessible at https://[your.domain]. If you used empty `data` folder, it will be initialized the first time you run the server.

If you used single-user config (file-root-pod.json), stop the server after the first run, open the config, and delete the section with email and password. You don't want your password to accidentally leak!

Get rid of this:

```json
    ,
    {
      "@id": "urn:solid-server:default:RootPodInitializer",
      "@type": "AccountInitializer",
      "email": "test@example.com",
      "password": "secret!"
    }
```

Now, the server is running. But when you reboot your VPS or the CSS server crashes, it will stop. We use pm2 to fix that.

[Next: Run CSS with pm2](use-pm2.md)
