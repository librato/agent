# Librato Agent

Release Notes for the Librato Agent

## Supported Platforms

* Debian 8 (Jessie)
* EL7 (RHEL, CentOS)
* EL6 (RHEL, CentOS)
* Amazon Linux 2015.09
* Ubuntu 15.10 (Wily)
* Ubuntu 15.04 (Vivid)
* ~~Ubuntu 14.10 (Utopic)~~ EOL
* Ubuntu 14.04 LTS (Trusty)
* Ubuntu 12.04 LTS (Precise)

## Releases

### 5.5.0-librato30

_Feb 24, 2016_

Instead of reporting absolute values for bytes ("complex") and inodes, we have disabled both of those and now report percentages by default instead. This change results in fewer, yet more useful, disk capacity metrics for the user.

### 5.5.0-librato29

_Jan 27, 2016_

Update the Docker plugin to accommodate the breaking Docker stats API changes for network metrics introduced in API version 1.21.

### 5.5.0-librato28

_Jan 12, 2016_

Adds support for our new NGINX Plus integration, with over 70 metrics courtesy of NGINX's [ngx_http_status_module](http://nginx.org/en/docs/http/ngx_http_status_module.html).

### 5.5.0-librato27

_Jan 8, 2016_

This release introduces container-level `blkio` (disk) metrics for Docker containers. The following metrics are now available:

#### Bytes

* librato.docker.blkio.io_service_bytes_async
* librato.docker.blkio.io_service_bytes_read
* librato.docker.blkio.io_service_bytes_sync
* librato.docker.blkio.io_service_bytes_total
* librato.docker.blkio.io_service_bytes_write

#### Operations

* librato.docker.blkio.io_serviced_async
* librato.docker.blkio.io_serviced_read
* librato.docker.blkio.io_serviced_sync
* librato.docker.blkio.io_serviced_total
* librato.docker.blkio.io_serviced_write

### 5.5.0-librato26

_Jan 7, 2016_

Adjust systemd configuration to allow `CAP_SETGID` and `CAP_SETUID`. This is needed for user/group privilege demotion by the Docker plugin.

### 5.5.0-librato25

_Dec 30, 2015_

Small bump to fix an inaccurate configuration comment.

### 5.5.0-librato24

_Dec 29, 2015_

Bump the default `Interval` setting to 60 seconds based on feedback from users.

### 5.5.0-librato23

_Dec 9, 2015_

Disable some Varnish settings that were causing warnings on older distributions. This plugin is not yet recognized as an official integration (no server-side preconfigurations) but metrics will be received by Librato if `librato.varnish.*` is added to the user-defined whitelist.

### 5.5.0-librato22

_Dec 8, 2015_

Fix `service` path for EL6.

### 5.5.0-librato21

_Dec 8, 2015_

Enable the `--deb-no-default-config-files` flag for all deb packages. Needed to keep our configuration in `/opt/collectd/etc` without warnings by Debian.

### 5.5.0-librato20

_Nov 13, 2015_

Officially support the Memcached plugin. The plugin has been patched to return `hostname_g` as the host rather than the service address. This adheres more closely to Librato's dynamic source convention. Also, the Node/Instance will be appended to the source.

### 5.5.0-librato19

_Nov 6, 2015_

Introducing support for EL7 and Ubuntu Wily.
