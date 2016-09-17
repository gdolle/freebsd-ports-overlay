# freebsd-ports-overlay

This repository is a port overlay. It aims to provide some ports missing to the FreeBSD official port tree. 
Note that some ports might not be well maintained!
Therefore you are allowed and encouraged to create your own (official/unoffical) ports based on this repository. :-)

# Prerequisites

In order to use the ports in this repository, you will have to install [portshaker(8)](http://www.freshports.org/ports-mgmt/portshaker/) and its configuration files, be sure you are selecting the HASKELL option:

```sh
# cd /usr/ports/ports-mgmt/portshaker-config
# make install clean
```

You can use the ports as usual after the process has finished.
You have to configure portshaker `/usr/local/etc/portshaker.conf`.

Note that if you use zfs, you can enable it adding `use_zfs="yes"` to `/usr/local/etc/portshaker.conf`. However
zfs mount points must exist beforehand for each portshaker directories

```
zfs create mypool/var/cache/
zfs create mypool/var/cache/portshaker
```
Then configure the compression to obtain the best performances.
The mount point `mypool/usr/ports` must exists.

# Install

We need to configure portshaker.

## First way 

Edit `/usr/local/etc/portshaker.conf`  and add a new repository via these two lines
```sh
gdolle_ports_tree="/usr/ports"
gdolle_merge_from="ports github:gdolle:freebsd_ports_overlay"
```
This will merge the system ports from portsnap, and this github repository.
Note that `gdolle` is just a prefix. Edit the variable `port_trees` to use this
configuration
```sh
ports_trees="gdolle"
```

Finally update portshaker and merge ports
```sh
portshaker -U
portshaker -M
```
The `/usr/ports` directory should be updated with the new merge.

Remark 1: In this example, the `/usr/local/etc/portshaker.d/github` script is used by parsing the `gdolle_merge_from=..` variable.

Remark 2: If you use zfs, the mountpoint `/usr/ports` must exist beforehand, otherwise the port trees prefix will be
used instead.

Remark 3: Currently, a password is asked each time, you might want to use the next method.

## Second way

Create a new file in `/usr/local/etc/portshaker.d/` for our new repository. We name it `github_gdolle` containing
```sh
#!/bin/sh
# $Id$

. /usr/local/share/portshaker/portshaker.subr

username="gdolle"
project="freebsd-ports-overlay"

if [ "$1" != '--' ]; then
        err 1 "Extra arguments"
fi
shift

method="git"
git_clone_uri="https://github.com/${username}/${project}"
git_branch=${git_branch:=master}

run_portshaker_command $*
```
then as before, we edit the `/usr/local/etc/portshaker.conf` file and add the new merge repositories

```sh
gdolle_ports_tree="/usr/ports"
gdolle_merge_from="ports gdolle_github"
```
Then we use the new configuration
```sh
ports_trees="gdolle"
```
Finally update portshaker and merge ports
```sh
portshaker -U
portshaker -M
```
The `/usr/ports` directory should be updated with the new merge now!
