# Namespaced WireGuard VPN

[![Copr build status][copr-status-image]][copr-restic-backup-package]

Systemd configuration for a backups using Restic


## Installation

This package is available from the
[chrisbouchard/upliftinglemma][upliftinglemma-project] Copr. Assuming you're
using a recent version of Fedora, you can install using `dnf`.

```console
$ dnf copr enable chrisbouchard/upliftinglemma
$ dnf install restic-backup
```

You may have to agree to some prompts if this is the first Copr you've enabled.


## Configuration

_TBW_

#### Configuration Values

- `TBW`:
  _TBW_


## Running

Once the configuration is done, you can enable backups with

```console
$ systemctl --user enable --now restic-backup.timer
$ systemctl --user enable --now restic-prune.timer
```


[copr-restic-backup-package]: https://copr.fedorainfracloud.org/coprs/chrisbouchard/upliftinglemma/package/restic-backup/
[copr-status-img]: https://copr.fedorainfracloud.org/coprs/chrisbouchard/upliftinglemma/package/restic-backup/status_image/last_build.png
[environmentfile=]: https://www.freedesktop.org/software/systemd/man/systemd.exec.html#EnvironmentFile=
[upliftinglemma-project]: https://copr.fedorainfracloud.org/coprs/chrisbouchard/upliftinglemma
