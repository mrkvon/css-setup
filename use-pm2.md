# Run CSS with pm2

We want the CSS to start automatically when server restarts, or when CSS crashes. We'll use [pm2](https://pm2.keymetrics.io/) to do that.

## Install pm2

```sh
sudo npm install -g pm2
```

Then, make sure it will start when server restarts

```sh
# run as [user]
pm2 startup
```

This command will output another command that you should execute. In my case it looks like this:

```sh
## don't do this!
## generate your own with `pm2 startup`! (run it as [user])
## run as [user] or root
# sudo env PATH=$PATH:/snap/node/7581/bin /usr/local/lib/node_modules/pm2/bin/pm2 startup systemd -u [user] --hp /home/[user]
```

## Run the server with pm2

### Create script to run with pm2

1. Create a file at `~/www/[projectname]/run.sh`:
   ```sh
   # run as [user]
   touch ~/www/[projectname]/run.sh
   ```
1. Open it with vim or nano, and paste your favourite way to run the CSS, for example:

   ```sh
   #!/bin/bash

   # put your favourite way to run CSS here, for example
   nvm exec 18 npx -y @solid/community-server@7 -f ./data/ -c ./config.json -p [port] -b https://[your.domain]
   ```

1. Make the script executable:

   ```sh
   # run as [user]
   chmod +x ~/www/[projectname]/run.sh
   ```

1. Test it
   ```sh
   # run as [user]
   # go to project folder
   cd ~/www/[projectname]
   # cd ~/www/[projectname]/CommunitySolidServer # if you run it from git repo
   # execute
   . ./run.sh
   ```

### Use pm2 to run the server

1. [Start CSS](https://pm2.keymetrics.io/docs/usage/quick-start/#start-an-app):
   ```sh
   # run as [user]
   cd ~/www/[projectname]
   # cd ~/www/[projectname]/CommunitySolidServer # if you run it from git repo
   pm2 start run.sh --name [name-like-domain.example]
   ```
1. Check that the server runs
1. Persist the settings
   ```sh
   # run as [user]
   pm2 save
   ```
1. Try to reboot your machine and see if CSS starts automatically

### Some useful commands to manage pm2

```sh
# each user manages its own pm2 instance

# see list of pm2 processes
pm2 list
# you'll see a list including name and id of each process

# see log of a particular process
pm2 log [name-or-id]

# start, restart, and stop process
pm2 start [name-or-id]
pm2 restart [name-or-id]
pm2 stop [name-or-id]

# see details about a process
pm2 describe [name-or-id]

# see nice overview of running processes
pm2 monit
```

[Next: Set up mashlib as default app](setup-mashlib.md)
