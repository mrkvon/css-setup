# Setup nginx

In this section we set up nginx and enable https using certbot and LetsEncrypt certificate

You should do this section as a `root`

## Prerequisity

Install nginx and make sure it's always running

```sh
# run as root or use sudo
# install
apt update
apt install nginx
# enable nginx to start on boot
systemctl enable nginx
# start the service
service nginx start
```

### Set up nginx

#### Create config

```sh
touch /etc/nginx/sites-available/[your.domain].conf
```

Fill it with the following

Replace

- `[your.domain]` &rArr; your domain name
- `[some-identifier]` &rArr; some unique identifier
- `[port]` &rArr; port number where CSS will run locally

It's a configuration to run CSS over http. We'll use Certbot later to change it into https setup.

```nginx
###### Setup for Community Solid Server ######
# https://solidproject.org/self-hosting/css/nginx

# Your local Solid server instance
upstream [some-identifier] {
  server 127.0.0.1:[port];
}

server {
  server_name [your.domain];
  listen 80;
  listen [::]:80; # this is for IPv6

  # Proxy traffic for https://[your.domain]/ to http://localhost:[port]/

  location / {
    # Delegate to the Solid server, passing the original host and protocol
    proxy_pass http://[some-identifier]$request_uri;
    proxy_set_header X-Forwarded-Host  $host;
    proxy_set_header X-Forwarded-Proto $scheme;

    # Pass these headers from the Solid server back to the client
    proxy_pass_header Server;
    proxy_pass_header Set-Cookie;

    # Enable Websocket support
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    # Prevent ETag modification (https://github.com/solid/community-server/issues/1036)
    gzip off;
  }
}
```

#### Finish http setup and test-run the server

Link your config to sites-enabled:

```sh
cd /etc/nginx/sites-enabled
ln -s /etc/nginx/sites-available/[your.domain].conf .
```

This creates a symbolic link from sites-available to sites-enabled. If you later decide to deactivate the server, this will allow you to delete the symbolic link from sites-enabled, while keeping the config safe in sites-available for future reference

Load the config:

```sh
service nginx reload
```

In other terminal, test-run the server on the `[port]`:

```sh
nvm exec 18 npx -y @solid/community-server@7 -p [port] -b http://[your.domain]
```

If you open `http://[your.domain]` in your browser, you should see the main page of Community Solid Server :tada:

Now, you can stop the CSS server in the other terminal with `Ctrl+C`

#### Set up https with Certbot

This is a critical step for the security and privacy of the server's users!

##### Install Certbot

Follow the [installation guide for Nginx and Debian at Certbot website](https://certbot.eff.org/instructions?ws=nginx&os=debiantesting)

##### Set up https

```sh
certbot --nginx
```

This will install the LetsEncrypt certificate, and edit your nginx config to support https.

You can check that the final config file at `/etc/nginx/sites-available/[your.domain].conf`. It will look [something like this]([your.domain].conf).

Certbot will automatically renew the certificates before they expire, so you don't have to worry about it.

Test the renewal by running

```sh
certbot renew --dry-run
```

##### Test https

```sh
nvm exec 18 npx -y @solid/community-server@7 -p [port] -b https://[your.domain]
```

Open `[your.domain]` in your browser and see that the server is running with `https:` at the beginning

You can stop the Community Solid Server in the other terminal with `Ctrl+C` for now. We'll set it up properly in next steps.

[Next: Set up the Community Solid Server properly](setup-css.md)
