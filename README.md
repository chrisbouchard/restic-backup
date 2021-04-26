# Restic Backup

[![Copr build status][copr-status-img]][copr-restic-backup-package]

Systemd configuration for a backups using Restic.


## Installation

This package is available from the
[chrisbouchard/upliftinglemma][upliftinglemma-project] Copr. Assuming you're
using a recent version of Fedora, you can install using `dnf`.

```console
$ dnf copr enable chrisbouchard/upliftinglemma
$ dnf install restic-backup
```

You may have to agree to some prompts if this is the first Copr you've enabled.


## Usage

There are two main timers and services in this package, and a couple support
services.

### `restic-backup.service`

This service is responsible for running the backup via `restic backup` and for
tagging backups to be deleted via `restic forget`. The `restic-backup.timer`
will run this service every night at midnight.

Note: This service won't actually delete any backups. That's handled by the
next service.

### `restic-prune.service`

This service deletes the backups that were marked for delete in the previous
service using `restic prune`. The `restic-prune.timer` will run this service
once a month at midnight on the first of the month.

### `restic-mount.service`

This service mounts your restic backups at the configured mount point (`restic
mount`) so you can explore your backups using the filesystem.

### `restic-notify-failure@.service`

This service is used by the others to report failures using `notify-send`.


## Running

Once the configuration is done, you can enable backups with

```console
$ systemctl --user enable --now restic-backup.timer
$ systemctl --user enable --now restic-prune.timer
```


## Configuration

Restic Backup is configured using a systemd [environment
file][environmentfile=], which by default is
`$XDG_CONFIG_DIR/restic-backup/restic-backup.conf`

### Configuration Values for `restic-backup.conf`

#### Backup Options

- `BACKUP_EXCLUDE_FILES`:
  List of exclude files, separated with colons (`:`). For more information
  about exclude files, see [the restic docs][restic-excluding].
  - Default: `$XDG_CONFIG_DIR/restic-backup/exclude`
- `BACKUP_EXCLUDE_IF_PRESENT`:
  List of exclude-if-present filenames, separated with colons (`:`). If any of
  these filenames are present in a directory, the directory will be ignored for
  backup.
  - Default: `.restic_exclude`
- `BACKUP_PATHS`:
  List of paths to include in the backups, separated with colons (`:`).
  - Default: `$HOME`
- `BACKUP_VERBOSE`:
  Verbosity level of the `restic backup` command.
  - Default: `0`

#### Forget Options

- `FORGET_DAILY`:
  Number of daily backups to keep. See [restic docs][restic-forget] for more
  information.
  - Default: _unset_
- `FORGET_MONTHLY`:
  Number of monthly backups to keep. See [restic docs][restic-forget] for more
  information.
  - Default: _unset_
- `FORGET_WEEKLY`:
  Number of weekly backups to keep. See [restic docs][restic-forget] for more
  information.
  - Default: _unset_
- `FORGET_YEARLY`:
  Number of yearly backups to keep. See [restic docs][restic-forget] for more
  information.
  - Default: _unset_
- `FORGET_VERBOSE`:
  Verbosity level of the `restic forget` command.
  - Default: `0`

#### Mount Options

- `MOUNT_PATH`:
  Mount point for `restic mount`.
  - Default: `$HOME/Restic`
- `MOUNT_VERBOSE`:
  Verbosity level of the `restic prune` command.
  - Default: `0`

#### Prune Options

- `PRUNE_VERBOSE`:
  Verbosity level of the `restic prune` command.
  - Default: `0`

#### Unlock Options

- `UNLOCK_VERBOSE`:
  Verbosity level of the `restic unlock` command.
  - Default: `0`


[copr-restic-backup-package]: https://copr.fedorainfracloud.org/coprs/chrisbouchard/upliftinglemma/package/restic-backup/
[copr-status-img]: https://copr.fedorainfracloud.org/coprs/chrisbouchard/upliftinglemma/package/restic-backup/status_image/last_build.png
[environmentfile=]: https://www.freedesktop.org/software/systemd/man/systemd.exec.html#EnvironmentFile=
[restic]: https://restic.net/
[restic-forget]: https://restic.readthedocs.io/en/stable/060_forget.html#removing-snapshots-according-to-a-policy
[restic-excluding]: https://restic.readthedocs.io/en/stable/040_backup.html#excluding-files
[upliftinglemma-project]: https://copr.fedorainfracloud.org/coprs/chrisbouchard/upliftinglemma
