# Setup

### DNS setup

Know the IPv4 and IPv6 address of your server.

Go to your Domain Registrar, and add A and AAAA DNS records pointing to your IPv4 and IPv6 address respectively. Wait a while for it to propagate; check the progress [here](https://www.whatsmydns.net/).

### Create linux user

`ssh` to your server as root (or run the commands with `sudo`)

Create a new Linux user for the purpose of running the CSS

```sh
# as root run
useradd -m -s /bin/bash [user]
```

### Prepare project folder structure

Switch to your new user and create a folder structure

Switch user

```sh
# as root run
su [user]
```

Go to home folder of [user]

```sh
# as [user] run
cd ~
```

Create project folder (feel free to choose different)

```sh
# as [user] run
mkdir -p ~/www/[projectname]
```

Ultimately, this is the folder structure we're aiming for:

```
www/[projectname]/
├── CommunitySolidServer/ (only if you're running CSS from github repository)
├── data/
├── mashlib/
├── pm2.sh
└── config.json
```

[Next: Install Community Solid Server](install-css.md)
