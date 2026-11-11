# virtualroot's OpenBSD ports

Personal work-in-progress OpenBSD ports tree. This is **not** an official
OpenBSD ports tree and these ports are **not** submitted to `ports@`; they
are held to the same quality bar (clean `portcheck`, proper `PLIST`/
`DESCR`, justified `WANTLIB`) as a permanent home, not a staging area.

Target: **OpenBSD 8.0, amd64**. Packages are release-coupled (shared
library versions in `@wantlib`), so these will not install on other
releases without rebuilding.

This repository licenses only the port Makefiles, patches and CI scripts
in it (see `LICENSE`, ISC) — each port's actual software keeps its own
upstream license (see each `Makefile`'s `PERMIT_PACKAGE` comment).

## Ports

| Port | Category | Upstream | License |
|---|---|---|---|
| [flux](sysutils/flux) | sysutils | [fluxcd/flux2](https://github.com/fluxcd/flux2) | Apache-2.0 |
| [talosctl](sysutils/talosctl) | sysutils | [siderolabs/talos](https://github.com/siderolabs/talos) | MPL-2.0 |
| [tinyauth](www/tinyauth) | www | [tinyauthapp/tinyauth](https://github.com/tinyauthapp/tinyauth) | AGPL-3.0 |

## Building from source

Clone this tree somewhere and point `PORTSDIR_PATH` at it in
`/etc/mk.conf` (clone it — a symlinked port directory is rejected by the
ports build system):

```
PORTSDIR_PATH=${PORTSDIR}:/home/you/ports
```

Then build and install normally:

```sh
cd /home/you/ports/sysutils/flux
make install
```

## Installing prebuilt packages

Packages are signed with [`signify`](https://man.openbsd.org/signify) and
published at `https://virtualroot.github.io/ports/` (served from this
repository's `gh-pages` branch).

Install the signing key once:

```sh
doas ftp -o /etc/signify/virtualroot-pkg.pub \
    https://raw.githubusercontent.com/virtualroot/ports/main/keys/virtualroot-pkg.pub
```

Then point `PKG_PATH` at the repository (matching your release and
architecture) and install normally:

```sh
export PKG_PATH="installpath:https://virtualroot.github.io/ports/pub/OpenBSD/$(uname -r)/packages/$(arch -s)/"
doas pkg_add flux talosctl tinyauth
```

## CI

- `port-test.yml` boots a real OpenBSD 8.0 VM and runs `make package`,
  `portcheck`, `port-lib-depends-check` and `update-plist` for any
  changed port on every push/PR.
- `publish.yml` builds, signs and publishes packages on a tag.
