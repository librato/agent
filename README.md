# Librato Agent

Release Notes for the Librato Agent

## Supported Platforms

* Amazon Linux 2016.03
* ~~Amazon Linux 2015.09~~ EOL
* Debian 8 (Jessie)
* Debian 7 (Wheezy)
* EL7 (RHEL, CentOS)
* EL6 (RHEL, CentOS)
* Ubuntu 15.10 (Wily)
* Ubuntu 15.04 (Vivid)
* ~~Ubuntu 14.10 (Utopic)~~ EOL
* Ubuntu 14.04 LTS (Trusty)
* Ubuntu 12.04 LTS (Precise)

## Releases

### 5.5.0-librato40

_Apr 25, 2016_

This release enables the native PostgreSQL queries to the Librato Agent. A number of "canned queries" are defined in `/opt/collectd/share/collectd/postgresql_default.conf` that make it easy to collect per-database statistics for memory use, indexing performance, tuple activity (inserts, updates, deletes, etc), commits and rollbacks, and much more.

The packaged configuration will attempt to gather statistics for the native `postgres` database using the UNIX socket. In most cases this will not work, since PostgreSQL uses [peer](http://www.postgresql.org/docs/9.5/static/auth-methods.html#AUTH-PEER) authentication for the UNIX socket. Adjust your PostgreSQL [authentication](http://www.postgresql.org/docs/9.5/static/auth-methods.html) or Librato Agent plugin settings in `/opt/collectd/etc/collectd.conf.d/postgresql.conf` as needed.

```
LoadPlugin postgresql
<Plugin postgresql>
  <Database postgres>
    Host "/var/run/postgresql"
    Port "5432"
    User "postgres"
  </Database>
  #<Database foo>
  #  Instance "custom-name"
  #  Host "127.0.0.1"
  #  Port "5432"
  #  User "username"
  #  Password "password"
  #  SSLMode "prefer"
  #</Database>
</Plugin>
```

The following queries are implicitly included for any `Database` block when no `Query` definitions are explicitly defined. Adding any explicit `Query` definitions will override the default behavior, so make sure to include the entire list below or any subsets as desired.

* `Query backends`
* `Query transactions`
* `Query queries`
* `Query query_plans`
* `Query table_states`
* `Query disk_io`
* `Query disk_usage`

### 5.5.0-librato39

_Apr 19, 2016_

**Update:** This release also introduces support for Debian 7 (wheezy).

**Update #2:** This release _also_ adds support for the i386 architecture with Debian 8 (jessie). This was an oversight on our part previously.

This release adds Elasticsearch monitoring capabilities to Librato Agent. Elasticsearch versions 2.x, 1.x and 0.90
are supported. A default configuration file is provided at `/opt/collectd/etc/collectd.conf.d/elasticsearch.conf`.
Please review and edit the following section to match your environment.

The plugin will attempt to connect to Elasticsearch over http://127.0.0.1:9200. Update the `Url` configuration
parameter to match the network.host setting in your Elasticsearch configuration file. By default, the cluster
name will serve as the plugin instance for the reported metrics. Use the `Name` configuration parameter to
override this value.

```
<Plugin "python">
    ModulePath "/opt/collectd/share/collectd/"

    Import "collectd-elasticsearch"

    <Module "collectd-elasticsearch">
        #Url "http://127.0.0.1:9200"
        #Name "mycluster"
        #Verbose true
    </Module>
</Plugin>
```

### 5.5.0-librato38

_Apr 18, 2016_

This release corrects a number of dependencies that were never moved from recommended or suggested to actual hard dependencies, particularly those libraries for python, mysqlclient, and memcached.

It also marks the introduction of support for Amazon Linux 2016.03. For all practical purposes the package should be universal across 2015.09 and 2016.03, but we're now officially treating 2015.09 as deprecated.

### 5.5.0-librato37

_Apr 8, 2016_

This release performs a "reset" of our Varnish configuration to disable a number of module classes providing metrics that are largely used only for debugging or deep troubleshooting. The resulting set of metrics is common across all supported platforms, meaning we no longer need specialized patches to support Varnish across different OS releases.

This also means we're getting closer to an official Varnish integration. :wink: 

### 5.5.0-librato36

_Mar 21, 2016_

This release enables the native MySQL plugin and patches it to report the following additional metrics.

* `librato.mysql.gauge.tmp_disk_tables` - Number of internal, on-disk temporary tables created
* `librato.mysql.gauge.tmp_files` - Number of temporary files created
* `librato.mysql.gauge.tmp_tables` - Number of internal (non-persistent) temporary tables
* `librato.mysql.counter.total_queries` - Number of statements executed, including stored procedures
* `librato.mysql.counter.slow_queries` - Number of queries that take more than the configured number of seconds
* `librato.mysql.gauge.mysql_uptime` - Number of seconds the server has been up
* `librato.mysql.counter.total_connections` - Number of connection attempts made to the server
* `librato.mysql.counter.aborted_clients` - Number of connection that weren't closed properly by the client
* `librato.mysql.counter.aborted_connects` - Number of failed attempts to connect to the server
* `librato.mysql.gauge.qcache_free_blocks` - Number of free memory blocks in the query cache
* `librato.mysql.gauge.qcache_total_blocks` - Total number of memory blocks in the query cache
* `librato.mysql.gauge.qcache_free_memory` - Amount of free memory in the query cache

A default configuration file also gets installed under `/opt/collectd/etc/collectd.conf.d/mysql.conf`, and looks as follows. Edit it according to your MySQL server deployment. Please see [MySQL Plugin](https://collectd.org/wiki/index.php/Plugin:MySQL) for more details.

```
LoadPlugin mysql
<Plugin "mysql">
  <Database "mydb">
    Host "127.0.0.1"
    User "root"
    Password ""
    Port 3306
    MasterStats true
    InnodbStats true
  </Database>
</Plugin>
```

### 5.5.0-librato35

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
