backup_servers
============

## Synopsis

Wrapper for rsync, parallelize backup.

This script is written in bash.

## Config
It use a simple bash config file (eg: /opt/backups/backup.conf):

- BACKUP_DIR=''
- SERVERS=''
- DIRS=''
- PARALLEL=''
- SSH_OPT=''
- CONN_TIMEOUT=''
- RSYNC_OPT=''

Or arguments on the command line:
  backup_servers --backup-dir /opt/backups --dirs /etc --servers my.server.com
  
The variable DIRS is use by runing `ls -1 DIRS` on the server via ssh.
This permit you to use any shell command to get the list of directories.

## Example

#/opt/backups/backup.conf
BACKUP_DIR=/opt/backups
SERVERS=$(mysql sys -NBe "SELECT host FROM server")
DIRS='if [ -e /etc/backup_paths ]; then cat /etc/backup_paths; else echo "/etc /home"'
RSYNC_OPT="--rsync-path="nice +19 rsync" --bwlimit 1024"

You can then test your config with arguments to change values of the config file:

  backup_servers --conf /opt/backups/backup.conf --servers macbook.brighton.loc --dirs /private/etc --debug
  
Or run the backup in rsync dry mode:
  
  backup_servers --conf /opt/backups/backup.conf -n
  

