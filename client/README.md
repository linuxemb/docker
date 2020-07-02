# docker-nfs-client

Docker image for a light NFS client (~10.9MB). By default NFS 4 is used.

Based on https://github.com/evq/nfs-client.

## Usage

### Build

    $ docker build -t nfs-client .

### Run

Basic example, mounting NFS within container:

    $ docker run -it --privileged=true --net=host -v /mnt/nfs-1 -e SERVER=192.168.0.9 -e SHARE=movies nfs-client

Writing back to the host:

    $ docker run -itd \
        --privileged=true \
        --net=host \
        --name nfs-movies \
        -v /media/nfs-movies:/mnt/nfs-1:shared \
        -e SERVER=192.168.0.9 \
        -e SHARE=movies \
          nfs-client

Take note of the historic 'NFS shares and volumes don't mix' ([#4213](https://github.com/docker/docker/issues/4213)) to use `shared` or `rshared` as needed when needing to use `--volumes-from` with other containers. Additionally, after the container is killed you'll need to unmount the host mount too.

#### Runtime Environment Variables

There should be a reasonable amount of flexibility using the available variables. If not please raise an issue so your use case can be covered!

- `SERVER` - the hostname of the NFS server to connect to
- `SHARE` - the name of the NFS share to mount
- `MOUNT_OPTIONS` - mount options to mount the NFS share with
- `FSTYPE` - the filesystem type; specify `nfs3` for NFSv3, default is `nfs` i.e. NFSv4
- `MOUNTPOINT` - the mount point for the NFS share within the container



```
