Installing GlusterFS - a Quick Start Guide
-------

### Deploy Using Ansible

If you are already an Ansible user, and are more comfortable with setting
up distributed systems with Ansible, we recommend you to skip all these and
move over to [gluster-ansible](https://github.com/gluster/gluster-ansible) repository, which gives most of the details to get the systems running faster.


### Installing GlusterFS

Install the software

```console
# dnf install glusterfs-server
```

Start the GlusterFS management daemon:

```console
# service glusterd start
```

```console
# gluster volume create gv0 replica 3 server1:/data/brick1/gv0 server2:/data/brick1/gv0 server3:/data/brick1/gv0
volume create: gv0: success: please start the volume to access data
# gluster volume start gv0
volume start: gv0: success
```

Confirm that the volume shows "Started":

```console
# gluster volume info
```

You should see something like this (the Volume ID will differ):

```console
Volume Name: gv0
Type: Replicate
Volume ID: f25cc3d8-631f-41bd-96e1-3e22a4c6f71f
Status: Started
Snapshot Count: 0
Number of Bricks: 1 x 3 = 3
Transport-type: tcp
Bricks:
Brick1: server1:/data/brick1/gv0
Brick2: server2:/data/brick1/gv0
Brick3: server3:/data/brick1/gv0
Options Reconfigured:
transport.address-family: inet
```

Note: If the volume does not show "Started", the files under
`/var/log/glusterfs/glusterd.log` should be checked in order to debug and
diagnose the situation. These logs can be looked at on one or, all the
servers configured.


### Step 7 - Testing the GlusterFS volume

For this step, we will use one of the servers to mount the volume.
Typically, you would do this from an external machine, known as a
"client". Since using this method would require additional packages to
be installed on the client machine, we will use one of the servers as
a simple place to test first , as if it were that "client".

```console
# mount -t glusterfs server1:/gv0 /mnt
# for i in `seq -w 1 100`; do cp -rp /var/log/messages /mnt/copy-test-$i; done
```

First, check the client mount point:

```console
# ls -lA /mnt/copy* | wc -l
```

You should see 100 files returned. Next, check the GlusterFS brick mount
points on each server:

```console
# ls -lA /data/brick1/gv0/copy*
```

You should see 100 files on each server using the method we listed here.
Without replication, in a distribute only volume (not detailed here), you
should see about 33 files on each one.

