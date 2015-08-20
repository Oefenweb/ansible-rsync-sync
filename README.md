## rsync-sync

[![Build Status](https://travis-ci.org/Oefenweb/ansible-rsync-sync.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-rsync-sync) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-rsync--sync-blue.svg)](https://galaxy.ansible.com/list#/roles/4814)

Perform synchronization using [rsync](https://rsync.samba.org/).

#### Requirements

* `rsync` (will be installed)

#### Variables

* `rsync_sync_install_path`: [default: `/usr/local/bin`]: Install directory

* `rsync_sync_scripts`: [default: `[]`]: Scripts declarations
* `rsync_sync_scripts.key`: [required]: The name of the script (e.g. `last-backup`)
* `rsync_sync_scripts.key.pre`: [optional, default `[]`]: Pre sync commands
* `rsync_sync_scripts.key.rsync`: [required]: Rsync declarations
* `rsync_sync_scripts.key.rsync.src`: [required]: The source directory
* `rsync_sync_scripts.key.rsync.dest`: [required]: The destination directory
* `rsync_sync_scripts.key.rsync.options`: [optional, default `[]`]: Options (e.g. `['--aP', '--delete']`)
* `rsync_sync_scripts.key.post`: [optional, default `[]`]: Post sync commands

## Dependencies

None

#### Example(s)

##### Synchronization a remote backup and import it

```yaml
---
- hosts: all
  roles:
   - rsync-sync
  vars:
    rsync_sync_scripts:
      last-backup:
        pre:
          - 'ssh -q -t example.com "test ! -f /var/lock/mydumper-backup"'
        rsync:
          src: '-e ssh example.com:/data/backup/mydumper/'
          dest: '/data/backup/mydumper'
          options:
            - '-aP'
            - '--delete'
        post:
          - "mydumper-restore"
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-rsync-sync/issues)!
