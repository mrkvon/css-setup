# Install Community Solid Server

## Prerequisity: run Node v18 or later

The latest release of Community Solid Server at the time of writing &mdash; version 7 &mdash; expects Node to be v18 or later

It's likely that the Node version on your Debian is lower (i have v16).

```sh
node -v
```

If it's less than 18, use NVM to switch to v18 or later

First [install NVM](https://github.com/nvm-sh/nvm?tab=readme-ov-file#install--update-script).

Install node v18 (or later)

```sh
# run as [user]
nvm install 18
```

## Install and test-run the server

### Option 1 - npx

No need to install, just run

```sh
# run as [user]
nvm exec 18 npx -y @solid/community-server -p [port]
```

### Option 2 - npm

Install the package globally

```sh
# run as [user] or root
nvm exec 18 sudo npm install -g @solid/community-server
```

Run the server

```sh
# run as [user]
community-solid-server -p [port]
```

### Option 3 - github repository

This is useful when you want to edit or debug the server code

Navigate to project folder, clone and install the project from [github repository](https://github.com/CommunitySolidServer/CommunitySolidServer)

```sh
# run as [user]
# navigate to project folder
cd ~/www/[projectname]
# clone from github
git clone https://github.com/CommunitySolidServer/CommunitySolidServer.git
# go to CSS folder
cd CommunitySolidServer
# install
nvm exec 18 npm ci
# run the server
nvm exec 18 npm start -- -p [port]
```

[Next: Set up nginx](setup-nginx.md)
