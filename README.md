backup_servers
=============

## Synopsis

Wrapper for rsync, parallelize backup. Used to backup 200 servers in production on usb disk 1To.

This script is written in bash.

## Config
It use a simple bash config file (eg: /etc/backup_servers.conf), here are the default values:

- BACKUP_DIR=""
- SERVERS=""
- DIRS=""
- PARALLEL=1
- RSYNC="rsync -au --links --force --numeric-ids --relative --devices --stats"
- RSYNC_OPT=""

Or arguments on the command line:

  `backup_servers --backup-dir /home/backup --dirs /etc --servers my.server.com`
  
The variable DIRS is used by runing `ls -1d DIRS` on the server via ssh.
Only exiting directories will be listed wich permit you to have one global directory policy for all servers.
You can use as well any `$(shell command)` to get the list of directories passed to `ls`.

## Example

```bash
  # /etc/backup_servers.conf:
  
  BACKUP_DIR=/home/backup
  PARALLEL=4
  SERVERS=$(mysql sys -NBe "SELECT host FROM server")
  # If you want specifics directories list per server, you can get it from a file on the server
  DIRS='$(if [ -e /etc/backup_servers.dirs ]; then cat /etc/backup_servers.dirs; else echo "/etc /home"; fi)'
  RSYNC_OPT="-z --rsync-path='nice +19 rsync' --bwlimit 1024"

```
You can then test your config with arguments to change values of the config file:

  `backup_servers --conf /etc/backup_servers.conf --servers myserver.mydomain.loc --dirs /etc --debug`
  
Or run the backup in rsync dry mode:
  
  `backup_servers --conf /etc/backup_servers.conf -n`
  

