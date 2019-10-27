### Getting Started

This tutorial will cover different options for getting a Gluster
cluster up and running. Here is a rundown of the steps we need to do.

To start, we will go over some common things you will need to know for
setting up Gluster. Then try installing Gluster. Finally, we will
install Gluster, create a few volumes, and test usingthem.

#### Common Terminology Used

First, it is important to understand that GlusterFS isn’t really a
filesystem in and of itself. It concatenates existing filesystems into
one (or more) big chunks so that data being written into or read out of
Gluster gets distributed across multiple hosts simultaneously. This
means that you can use space from any host that you have available.

Typically, XFS is recommended but it can be used with other filesystems
as well. 

-   A **trusted pool** refers collectively to the hosts in a given
    Gluster Cluster.
-   A **node** or “server” refers to any server that is part of a
    trusted pool. In general, this assumes all nodes are in the same
    trusted pool.
-   A **brick** is used to refer to any storage device (really this
    means filesystem) that is being used for Gluster export.
-   An **export** refers to the mount path of the brick(s) on a given
    server, for example, /export/brick1
-   A **Gluster volume** is a collection of one or more bricks.
    This is analogous to /etc/exports entries for NFS.

