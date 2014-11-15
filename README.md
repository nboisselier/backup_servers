backup_servers
============

## Synopsis

Wrapper for rsync, parallelize backup.

It use a simple bash config file (eg: /opt/backups/backup.conf):

- SERVERS=''
- DIRS=''
- PARALLEL=''
- SSH_OPT=''
- CONN_TIMEOUT=''
- RSYNC_OPT=''


## Example

You can then test your config with arguments to change values of the config file:

  backup_servers --conf /opt/backups/backup.conf --servers macbook.brighton.loc --dirs /private/etc --debug

