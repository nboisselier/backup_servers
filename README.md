backup_servers
=============

## Synopsis

Wrapper for rsync, parallelize backup. Used to backup 200 servers.

This script is written in bash.

## Config
It use a simple bash config file (eg: /home/backup/backup.conf):

- BACKUP_DIR=""
- SERVERS=""
- DIRS=""
- PARALLEL=1
- RSYNC="rsync -au --links --force --numeric-ids --relative --devices --stats"
- RSYNC_OPT=""
- SSH_OPT=""
- CONN_TIMEOUT=10 # seconds
- RUN_TIMEOUT=0 # seconds

Or arguments on the command line:

  `backup_servers --backup-dir /home/backup --dirs /etc --servers my.server.com`
  
The variable DIRS is used by runing `ls -1 DIRS` on the server via ssh.
Only exiting directories will be listed wich permit you to have one global directory policy for all servers.
You can use as well any `$(shell command)` to get the list of directories passed to `ls`.

## Example

```bash
  # /home/backup/backup.conf:
  
  BACKUP_DIR=/home/backup
  PARALLEL=4
  SERVERS=$(mysql sys -NBe "SELECT host FROM server")
  DIRS='$(if [ -e /etc/backup_paths ]; then cat /etc/backup_paths; else echo "/etc /home"; fi)'
  RSYNC_OPT="-zau --rsync-path='nice +19 rsync' --bwlimit 1024"

```
You can then test your config with arguments to change values of the config file:

  `backup_servers --conf /home/backup/backup.conf --servers macbook.brighton.loc --dirs /private/etc --debug`
  
Or run the backup in rsync dry mode:
  
  `backup_servers --conf /home/backup/backup.conf -n`
  

