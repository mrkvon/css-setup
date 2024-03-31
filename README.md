# Running Community Solid Server with Debian, pm2, and nginx

Here we document one possible way to set up Community Solid Server (CSS), including https and default app &mdash; Mashlib.

Steps:

1. [Own a domain name and point it to the server's IP address](initial-setup.md#dns-setup)
1. [Create a dedicated Linux user](initial-setup.md#create-linux-user)
1. [Prepare project folder structure](initial-setup.md#prepare-project-folder-structure)
1. [Install Community Solid Server](install-css.md)
1. [Set up nginx, and certificates with LetsEncrypt](setup-nginx.md)
1. [CSS Configuration](setup-css.md)
1. [Run CSS with pm2](use-pm2.md)
1. [Set up Mashlib](setup-mashlib.md)
1. [Backup data regularly with cron](backup.md)

## Prerequisities

You'll need:

- a domain name
- a server with Debian 11 and static IP address, VPS from a server provider will do
- Node and npm installed, maybe also NVM (Node Version Manager)
- pm2 installed
- nginx installed
- ftp server to backup the data

## Conventions

### Placeholders

We mark placeholders `[placeholder]`. You should always replace them with your own string. For example replace `[port]` with `3456`. Whatever you choose instead of the placeholders, keep it consistent across this whole instruction.

### Linux user

You need to run some commands as `root`, and others as regular `[user]`. Make sure you always run them as the correct user. It matters a lot! You may even want to open 2 ssh connections to your server &mdash; one as `[user]`, the other as `[root]`.

Look for `# run as [user]` or `# run as root` at the beginning of the script snippets.

[Next: Let's get started with the setup](initial-setup.md)
