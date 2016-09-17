# freebsd-ports-overlay

# Prerequities

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

# Install

We need to configure portshaker.

## First way 

Edit `/usr/local/etc/portshaker.conf`  and add a new repository via these two lines
```sh
gdolle_ports_tree="/usr/ports"
gdolle_merge_from="github:gdolle:freebsd_ports_overlay"
```
Note that `gdolle` is a prefix. Add this new repository to the ports tree 
which will be merged. Edit the variable `port_trees` and add the prefix to the existing ones.
```sh
ports_trees="prefix1 prefix2 ... gdolle"
```

Finally update portshaker and merge ports
```sh
portshaker -U
portshaker -M
```

Remark 1: In this example, the `/usr/local/etc/portshaker.d/github` script is used by parsing the `gdolle_merge_from=..` variable.

Remark 2: Currently, a password is asked each time, you might want to use the next method.

## Second way

Create a new file in `/usr/local/etc/portshaker.d/` for our new repository. We name it `github_gdolle` containing
```sh
#!/bin/sh
# $Id$

. /usr/local/share/portshaker/portshaker.subr

username="gdolle"
project="freebsd-ports-overlay"

extra_info=":${username}:${project}"

if [ "$1" != '--' ]; then
        err 1 "Extra arguments"
fi
shift

method="git"
git_clone_uri="https://github.com/${username}/${project}"
git_branch=${git_branch:=master}

run_portshaker_command $*
```
then as before, we edit the `/usr/local/etc/portshaker.conf` file and add the repository

```sh
gdolle_ports_tree="/usr/ports"
gdolle_merge_from="gdolle_github"
```
Then we add it to the existing port trees
```sh
ports_trees="prefix1 prefix2 ... gdolle"
```
Finally update portshaker and merge ports
```sh
portshaker -U
portshaker -M
```
