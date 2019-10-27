## Adminstrator's handbook

This document covers basic of different tasks required to manage GlusterFS storage. Depending on how you are using gluster (ie, ansible, in k8s as operator), these sections need not be required for you.

While each section can be expanded into larger chapter, go get started, and to manage 80-90% of the tasks, these sections would help you.

### Managing the glusterd Service

After installing GlusterFS, you must start glusterd service. The
glusterd service serves as the Gluster elastic volume manager,
overseeing glusterfs processes, and co-ordinating dynamic volume
operations, such as adding and removing volumes across multiple storage
servers non-disruptively.

```
$ sudo chkconfig glusterd enable
$ sudo service glusterd start
```


### Create a 'Trusted Storage Pool'

A trusted storage pool(TSP) is a trusted network of storage servers.
Before you can configure a GlusterFS volume, you must create a trusted
storage pool of the storage servers that will provide bricks to the
volume by peer probing the servers. The servers in a TSP are peers
of each other.

After installing Gluster on your servers and before creating a trusted
storage pool, each server belongs to a storage pool consisting of only
that server. A new peer can only be added to the pool by running below commands from existing pool, and not from a new node.

```

$ sudo gluster peer probe <new-node>    # Add a new node to pool
$ sudo gluster peer status              # Show the status of each connection
$ sudo gluster pool list                # list all peers and their ID.
$ sudo gluster peer detach <peer-name>  # Detach a peer from pool

```

### Setting up Storage to export

GlusterFS doesn't impose a strict restriction on how or what your
storage filesystem should be like. It has just one requirement, which
is the backend filesystem should support **Extended Attributes**
(xattrs).

If one wants **'Snapshot'** support in Gluster volume, then it should
be decided at setting up time, and LVM based storage should be used as
storage on backend.

We recommend not to use the device root itself as the brick, but use
a directory inside as gluster brick. For example, if you have mounted
`/data/export1` as storage with 10TB which needs to be exported,
consider giving `node:/data/export1/voldir` as the brick during volume
create.

### Creating Volumes

There are multiple volume types, and we recommend you to pick the
right volume type based on usecase. For the scope of this document we will list only replicated volume creation.

#### Creating Replicated Volumes

Replicated volumes create copies of files across multiple bricks in the
volume. You can use replicated volumes in environments where
high-availability and high-reliability are critical.

> **Note**:
> The number of bricks should be equal to of the replica count for a
> replicated volume. To protect against server and disk failures, it is
> recommended that the bricks of the volume are from different servers.

![replicated_volume](https://cloud.githubusercontent.com/assets/10970993/7412379/d75272a6-ef5f-11e4-869a-c355e8505747.png)

**To create a replicated volume**

1.  Create a trusted storage pool.

2.  Create the replicated volume:

    `# gluster volume create  [replica ] `

    For example, to create a replicated volume with two storage servers:

        # gluster volume create test-volume replica 3 transport tcp server1:/exp1/gluster server2:/exp2/gluster
        Creation of test-volume has been successful
        Please start the volume to access data.


#### Arbiter configuration for replica volumes

Arbiter volumes are replica 2 volumes with an additional export where the 3rd brick acts as the arbiter brick. This configuration has mechanisms that prevent occurrence of split-brains.

It can be created with the following command:

    `# gluster volume create  <VOLNAME>  replica 2 arbiter 1 host1:brick1 host2:brick2 host3:brick3`

More information about this configuration can be found at *Features : afr-arbiter-volumes*

Note that the arbiter configuration for replica 3 can be used to create distributed-replicate volumes as well.


### Start the volume

Starting the volume will start `glusterfsd` process, which exports the storage on all the nodes where there are bricks. Make sure you start your volumes before you try to mount them or else client operations after the mount will hang.


### Start the client

To access the Gluster Storage setup above, you need to `mount` the volume.

#### Install client packages.

Most of the distributions come with glusterfs client packages. If not, you can install it with your package manager. On Fedora systems, you would do:

```
$ sudo dnf install glusterfs glusterfs-client
$ man mount.glusterfs

```

#### Mounting the volume

```
$ sudo mount -t glusterfs server1:/test-volume /mnt/glusterfs`
$ 
$ mount | grep /mnt/glusterfs   # Check if mount is visible
$ df -h /mnt/glusterfs          # Check if all the storage is visible

```

NOTE: You may need to open up FireWall rules (ports 24007, 49152+) to make sure you can connect to all gluster servers.


With the mount successfully working, you and your users can consume GlusterFS storage.
