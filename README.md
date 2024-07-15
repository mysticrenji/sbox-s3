# SBOX-S3

SBOX-S3 is a self-hosted S3 object storage solution built using the S3 API from [SeasweedFS](https://seaweedfs.github.io/) 
and can be run on an inexpensive VPS like from [netcup](https://www.netcup.de/?ref=223843), 
[Hetzner](https://www.hetzner.com/cloud) or [contabo.com](https://contabo.com/en/vps/).

For storage space, any mountable remote storage can be used. I am using a [Hetzner Storagebox](https://www.hetzner.com/storage/storage-box).
The remote storage is mounted into the VPS using [Rclone](rclone.org). 
Thanks to Seaweedfs and Rclones clever caching, you get very fast S3 object storage that allows
you to store and retrieve data/objects in a scalable and cost-effective manner. 

## Prerequisites

To run SBOX-S3, you'll need the following:

- A VPS with at least 2 cores and 2GB of RAM. A VPS with a fast NVME SSD is recommended.
- A Hetzner Storagebox for storage or any remote storage supported by rclone. 
  
## Configuration

To configure SBOX-S3 modify the files `rclone.conf`, `s3.conf` and `docker-compose.yml`.

1. Modify the `rclone.conf`file to configure your remote storage.
2. Modify the `s3.conf` file to configure your s3 credentials.
3. Adjust the mount point of your data folder in the `docker-compose.yml` file (`-./data:data:shared`). Currently, the default mount point is `./data`. This means, a data folder will be created in the current directory with the name `data`. 
4. Remove/comment exposed ports from the `docker-compose.yml` file (`ports:`) if you don't need them. 
    - Ports:
        - s3 API: 8333 
        - Filer: 8888 (grpc: 18888)
        - Volume: 8080
        - Master: 9333


## Deployment

To deploy SBOX-S3, follow these steps:

1. Clone the SBOX-S3 repository.
2. Configuration (see above).
3. Run `docker-compose up -d` to start the container.


## Usage

Once SBOX-S3 is deployed, you can use the S3 API (e.g via rclone, aws cli, minio client, and any other s3 sdks) to interact with the object storage. Refer to the documentation for the seasweedfs S3 API for more information on how to use it.

#### Rclone mount

You can use rclone to mount the SBOX-S3 to your local machine.
1. Install rclone on your local machine.
2. Run `rclone config` to configure rclone.
3. Run `rclone mount` 


#### SeaweedFS mount
Ypu can use the SeaweedFS mount to mount the rSBOX-S3 to your local machine.
1. Install the SeaweedFS  on your local machine.
2. Run `weed mount --filter=host:8888 ---volumeServerAccess=filerProxy -dir=/some/dir` 



## Contributing

Contributions are welcome! If you have any ideas, suggestions, or bug reports, please open an issue or submit a pull request.

## License

This project is licensed under the [MIT License](LICENSE).
