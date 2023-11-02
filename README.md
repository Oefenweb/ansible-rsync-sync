## rsync-sync

[![CI](https://github.com/Oefenweb/ansible-rsync-sync/workflows/CI/badge.svg)](https://github.com/Oefenweb/ansible-rsync-sync/actions?query=workflow%3ACI)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-rsync--sync-blue.svg)](https://galaxy.ansible.com/Oefenweb/rsync_sync)

Perform synchronization using [rsync](https://rsync.samba.org/).

#### Requirements

* `rsync` (will be installed)

#### Variables

* `rsync_sync_install_path`: [default: `/usr/local/bin`]: Install directory

* `rsync_sync_scripts`: [default: `{}`]: Scripts declarations
* `rsync_sync_scripts.key`: [required]: The name of the script (e.g. `last-backup`)
* `rsync_sync_scripts.key.pre`: [optional, default `[]`]: Pre sync commands
* `rsync_sync_scripts.key.rsync`: [required]: Rsync declarations
* `rsync_sync_scripts.key.rsync.src`: [required]: The source directory
* `rsync_sync_scripts.key.rsync.dest`: [required]: The destination directory
* `rsync_sync_scripts.key.rsync.options`: [optional, default `[]`]: Options (e.g. `['--aP', '--delete']`)
* `rsync_sync_scripts.key.rsync.time`: [optional, default `false`]: Whether or not to time the `rsync` command
* `rsync_sync_scripts.key.post`: [optional, default `[]`]: Post sync commands

* `rsync_sync_x_from_files`: [optional, default `[]`]:
* `rsync_sync_x_from_files.{n}.path`: [required]: The remote path of the file (relative to `/etc/rsync-sync`)
* `rsync_sync_x_from_files.{n}.owner`: [optional, default `root`]: The name of the user that should own the file
* `rsync_sync_x_from_files.{n}.group`: [optional, default `root`]: The name of the group that should own the file
* `rsync_sync_x_from_files.{n}.mode`: [optional, default `0644`]: The UNIX permission mode bits of the file
* `rsync_sync_x_from_files.{n}.content`: [optional, default `[]`]: File content lines

* `rsync_sync_jobs`: [default: `[]`]: Sync jobs (scheduled by `cron.d`)
* `rsync_sync_jobs.{n}.name`: [required]: Description of a crontab entry, should be unique, and changing the value will result in a new cron task being created (e.g. `last-backup`)
* `rsync_sync_jobs.{n}.job`: [required]: The command to execute (e.g. `/usr/local/bin/last-backup`)
* `rsync_sync_jobs.{n}.state`: [default: `present`]: Whether to ensure the job is present or absent
* `rsync_sync_jobs.{n}.day`: [default: `*`]: Day of the month the job should run (`1-31`, `*`, `*/2`)
* `rsync_sync_jobs.{n}.hour`: [default: `*`]: Hour when the job should run (e.g. `0-23`, `*`, `*/2`)
* `rsync_sync_jobs.{n}.minute`: [default: `*`]: Minute when the job should run (e.g. `0-59`, `*`, `*/2`)
* `rsync_sync_jobs.{n}.month`: [default: `*`]: Month of the year the job should run (e.g `1-12`, `*`, `*/2`)
* `rsync_sync_jobs.{n}.weekday`: [default: `*`]: Day of the week that the job should run (e.g. `0-6` for Sunday-Saturday, `*`)

## Dependencies

None

#### Example(s)

##### Synchronization a remote backup and import it

```yaml
---
- hosts: all
  roles:
   - oefenweb.rsync-sync
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
          - mydumper-restore
    rsync_sync_x_from_files:
      - path: last-backup-include
        content:
          - |
            + *_app.*videos-schema.sql
            + *_app.*videos.sql

            + *_app_archive.*.sql
            + *_app_archive.*-schema.sql

            + metadata

            - *
    rsync_sync_jobs:
      - name: last-backup
        job: /usr/local/bin/last-backup
        minute: 0
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-rsync-sync/issues)!
