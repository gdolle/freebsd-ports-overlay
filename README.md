# freebsd-ports-overlay


# Install

In order to use the ports in this repository, you will have to install [portshaker(8)](http://www.freshports.org/ports-mgmt/portshaker/) and its configuration files, be sure you are selecting the HASKELL option:

```sh
# cd /usr/ports/ports-mgmt/portshaker-config
# make install clean
```

Start [portshaker(8)](http://www.freshports.org/ports-mgmt/portshaker/), and answer the questions.

```sh
# portshaker -v
```

You can use the ports as usual after the process has finished.
