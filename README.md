# Docker image for SSDB

This repository contains Dockerfile to build an image that dock [SSDB](http://ssdb.io/) service.

## Usage

### Build this image

The pre-built image is publish in Docker Hub with name `r3v3r/ssdb`

To build it yourself, run this command on project directory:
```sh
$ docker build -t <your_image_name_here>[:<tag name>] .
```

Example: Build an image from this Dockerfile with name `rever/ssdb` and tag `latest`
```sh
$ docker build -t rever/ssdb:latest .
```
### Run service
Run with default configuration:
```sh
$ docker run -d -p 8888:8888 r3v3r/ssdb 
```
NOTE: Replace `-p 8888:8888` with `-p <your_external_port>:8888`

Run with custom configuration
```sh
$ docker run -d -p <your_external_port>:8888 \
 [-v <folder_contains_config_file_on_host>:<folder_contains_config_file_on_container>] \
 [-v <data_folder_on_host>:<data_folder_on_container>:rw] \
 [-v <log_folder_on_host>:<log_folder_on_container>:rw] \
 [--name <your_container_name>] \
 r3v3r/ssdb <folder_contains_config_file_on_container>/<config_file_name>
```
NOTE: `<data_folder_on_container>` must be the same with `work_dir` in config file; and, `<log_folder_on_container>` must be the same with folder contain log files (`logger.output`) in config file


Example:
```sh
$ docker run -d -p 8888:8888 \
	-v /root/docker-ssdb/conf:/etc/ssdb \
	-v /root/docker-ssdb/data:/var/lib/ssdb:rw \
	-v /root/docker-ssdb/log:/var/log/ssdb:rw \
	--name test_ssdb \
	r3v3r/ssdb /etc/ssdb/ssdb.conf
```
This command will spawn a docker container that run ssdb service with these configurations:
 * Configuration file in /root/docker-ssdb/conf/ of host machine
 * Configuration name is `ssdb.conf`
 * Data of ssdb will be storaged in `/root/docker-ssdb/data` of host machine
 * Log files will be storaged in `/root/docker-ssdb/log` of host machine`
