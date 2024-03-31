# Backup the data

Everything is up and running now. Great! :tada:

What's left is regular backup of the user data in case server breaks down.

## One of the options

My server is a bit specific. I use rsync + stunnel to securely send data to a backup ftp server. Note: rsync on its own sends data insecurely. Here's an [instruction](https://help.wedos.cz/navody/wedos-disk/wedos-disk-sifrovane-pripojeni-pres-rsync/) I used (in Czech).

My backup script performs the work in `/home/[user]/[path-to-backup]/` folder, and it looks like this:

`backup.sh`

```sh
# archive the data folder
tar -czf /home/[user]/[path-to-backup]/$PERIOD-backup.tar.gz /home/[user]/www/[projectname]/data/
# encrypt the archive with gpg
gpg -r [public-key-identifier] --trust-model always --encrypt /home/[user]/[path-to-backup]/$PERIOD-backup.tar.gz
# send the encrypted archive over to remote server with rsync + stunnel
rsync -r /home/[user]/[path-to-backup]/$PERIOD-backup.tar.gz.gpg rsync://s21445@localhost/s21445/[path-to-remote-backup]/ --password-file=[/path/to/password-file.pass]
# clean up
rm /home/[user]/[path-to-backup]/$PERIOD-backup.tar.gz /home/[user]/[path-to-backup]/$PERIOD-backup.tar.gz.gpg
```

I use cron to run this script regularly:

```cron
# m h  dom mon dow command
30  01 *   *   *   PERIOD=daily /home/[user]/[path-to-backup]/backup.sh
35  02 *   *   2   PERIOD=weekly /home/[user]/[path-to-backup]/backup.sh
40  03 5   *   *   PERIOD=monthly /home/[user]/[path-to-backup]/backup.sh
```

This stores 3 backups from different times. You can specify your own days and times of backup.

All this is done as `[user]`. Not `root`.

You can figure out your own way. Make sure the data are sent and stored securely, and that you have a way to access and decrypt them in case something goes wrong!

---

Thank you for reading and trying this out.

That's all for now. I hope your server is running to your satisfaction. I may add some further instructions, like

- How to run CSS for a single user with custom WebID

If you find any mistakes, or have ideas for improvement, please [open an issue on GitHub](https://github.com/mrkvon/css-setup/issues).

Best wishes!

<!--[Bonus: How to run CSS for a single user with custom WebID](custom-webid.md)-->

<!--[Bonus: Migration from CSS v6 to v7](migrate-css.md)-->
