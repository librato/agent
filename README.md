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

### 5.5.0-librato34

_Mar 17, 2016_

This release enables support for the HAProxy plugin. The plugin has also been patched to prefix metrics with `backend` and `frontend`, depending on the source of the metrics and the proxy type. Health-specific metrics, such as `uptime_seconds`, are not prefixed. This adheres more closely to Librato's dynamic source convention. We are also appending the name of the proxy to the source.

### 5.5.0-librato34

_Mar 4, 2016_

This release enables the native Zookeeper plugin. By default, the plugin will look for a Zookeeper service running on localhost TCP port 2181. Change this by editing `/opt/collectd/etc/collectd.conf.d/zookeeper.conf`, as shown below.

```
LoadPlugin zookeeper
<Plugin "zookeeper">  
   Host "127.0.0.1"
   Port "2181"
</Plugin>
```

### 5.5.0-librato33

_Mar 3, 2016_

Officially support the Apache plugin. The plugin has been patched to return hostname_g as the host rather than the service address. This adheres more closely to Librato's dynamic source convention. Also, the Node/Instance will be appended to the source.

### 5.5.0-librato32

_Mar 2, 2016_

The previous release added five new metrics to track overall container state and image counts within Docker. Unfortunately it would report one copy of each metric (stream) for every container running on the host, resulting in N streams instead of 1 global (hostname) stream per metric. This release fixes that oversight.

### 5.5.0-librato31

_Feb 26, 2016_

This release introduces a handful of new Docker metrics for tracking container counts by state and total image count.

* `librato.docker.images.total`
* `librato.docker.containers.total`
* `librato.docker.containers.running`
* `librato.docker.containers.stopped`
* `librato.docker.containers.paused`

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
