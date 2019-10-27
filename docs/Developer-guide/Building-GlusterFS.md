# Building GlusterFS
This page describes how to build and install GlusterFS.

Build Requirements
------------------

GlusterFS depends on many open source packages, and to build it from
source, you need to have those dependencies taken care of.

### Dependencies

The following dnf command installs all the build requirements for
Fedora,

```console
# dnf install automake autoconf libtool flex bison openssl-devel  \
    libxml2-devel python3-devel libaio-devel readline-devel \
    lvm2-devel glib2-devel userspace-rcu-devel libcmocka-devel \
    libacl-devel fuse-devel libtirpc-devel make
```

If there are missing dependencies, the `./configure` step would fail,
and let you know. Follow the instruction to get those dependencies
resolved.

### Get the source

You can get the source using `git clone https://github.com/gluster/glusterfs`. After cloning, do `cd glusterfs/` to get into the source directory.

### Build

Run autogen to generate the configure script.

```console
$ ./autogen.sh
```

Once autogen completes successfully a configure script is generated.
Run the configure script to generate the makefiles.

```console
$ ./configure
```

If the above build requirements have been installed, running the
configure script should give the below configure summary,

```
GlusterFS configure summary
===========================
FUSE client          : yes
epoll IO multiplex   : yes
fusermount           : yes
readline             : no
georeplication       : yes
Linux-AIO            : no
Enable Debug         : no
Enable ASAN          : no
Enable TSAN          : no
Use syslog           : yes
XML output           : yes
Unit Tests           : no
Track priv ports     : yes
POSIX ACLs           : yes
SELinux features     : yes
firewalld-config     : no
Events               : yes
EC dynamic support   : x64 sse avx
Use memory pools     : yes
Nanosecond m/atimes  : yes
Server components    : yes
Legacy gNFS server   : no
IPV6 default         : no
Use TIRPC            : yes
With Python          : 3.7
Cloudsync            : yes
```

There are many options for GlusterFS during `./configure` time. Best is to check `./configure --help` and see what does the latest code give as options.

```
  --enable-debug          Enable debug build options.
  --enable-asan           Enable Address Sanitizer support
  --enable-tsan           Enable ThreadSanitizer support
  --disable-privport_tracking
                          Disable internal tracking of privileged ports.
  --enable-valgrind       Enable valgrind for resource leak debugging.
  --disable-fuse-client   Do not build the fuse client. NOTE: you cannot mount
                          glusterfs without the client
  --disable-fusermount    Use system's fusermount
  --disable-epoll         Use poll instead of epoll.
  --disable-georeplication
                          Do not install georeplication components
  --disable-events        Do not install Events components
  --enable-firewalld      enable installation configuration for firewalld
  --disable-xml-output    Disable the xml output
  --disable-selinux       Disable SELinux features
  --disable-mempool       Disable the Gluster memory pooler.
  --disable-syslog        Disable syslog for logging
  --enable-gnfs           Enable legacy gnfs server xlator.
  --enable-cmocka         Enable cmocka build options.
  --disable-ec-dynamic    Disable all dynamic code generation extensions for
                          EC module
  --disable-ec-dynamic-intel
                          Disable all INTEL dynamic code generation extensions
                          for EC module
  --disable-ec-dynamic-arm
                          Disable all ARM dynamic code generation extensions
                          for EC module
  --disable-ec-dynamic-x64
                          Disable dynamic INTEL x64 code generation for EC
                          module
  --disable-ec-dynamic-sse
                          Disable dynamic INTEL SSE code generation for EC
                          module
  --disable-ec-dynamic-avx
                          Disable dynamic INTEL AVX code generation for EC
                          module
  --disable-ec-dynamic-neon
                          Disable dynamic ARM NEON code generation for EC
                          module
```

Once configured, GlusterFS can be built with a simple make command.

```console
$ make
```

Run `make install` to install GlusterFS. By default, GlusterFS will be
installed into `/usr/local` prefix. To change the install prefix, give
the appropriate option to configure. If installing into the default
prefix, you might need to use 'sudo' or 'su -c' to install.

```console
# sudo make install
```

NOTE: glusterfs can be installed on any target path. However, the
`mount.glusterfs` script has to be in `/sbin/mount.glusterfs` for
mounting via command `mount -t glusterfs` to work. See -t section
in man 8 mount for more details.


### Running GlusterFS

GlusterFS can be only run as **root** user, so the following commands
will need to be run as root. A source install will generally not
install any init scripts. So you will need to start glusterd manually.
To manually start glusterd just run,

```console
# glusterd
```

This will start glusterd and fork it into the background as a daemon
process. You now run `gluster` commands and continue to create and
manage volumes as per the [admin guide](#)

## What to contribute?

There are many ways one can contribute to this project. Below are some
sections we have listed, which if it interests you, would help you get
started. If your interest areas are not listed, contact us to check
how else you can contribute?

### Documentation

Documentation is very critical for any project to succeed.

## How to contribute?