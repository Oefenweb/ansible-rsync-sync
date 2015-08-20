## rsync-sync

[![Build Status](https://travis-ci.org/Oefenweb/ansible-rsync-sync.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-rsync-sync) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-rsync--sync-blue.svg)](https://galaxy.ansible.com/list#/roles/4814)

Perform synchronization using [rsync](https://rsync.samba.org/).

#### Requirements

* `rsync` (will be installed)

#### Variables

* `rsync_sync_install_path`: [default: `/usr/local/bin`]: Install directory

* `rsync_sync_scripts`: [default: `[]`]: Scripts declarations

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
