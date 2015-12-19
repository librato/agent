# Librato Agent

Release Notes for the Librato Agent

## Supported Platforms

* Debian 8 (Jessie)
* EL7 (RHEL, CentOS)
* EL6 (RHEL, CentOS)
* Ubuntu 15.10 (Wily)
* Ubuntu 15.04 (Vivid)
* ~~Ubuntu 14.10 (Utopic)~~ EOL
* Ubuntu 14.04 LTS (Trusty)
* Ubuntu 12.04 LTS (Precise)

## Releases

### 5.5.0-librato23

Disable some Varnish settings that were causing warnings on older distributions. This plugin is not yet recognized as an official integration (no server-side preconfigurations) but metrics will be received by Librato if `librato.varnish.*` is added to the user-defined whitelist.

### 5.5.0-librato22

Fix `service` path for EL6.

### 5.5.0-librato21

Enable the `--deb-no-default-config-files` flag for all deb packages. Needed to keep our configuration in `/opt/collectd/etc` without warnings by Debian.

### 5.5.0-librato20

Officially support the Memcached plugin. The plugin has been patched to return `hostname_g` as the host rather than the service address. This adheres more closely to Librato's dynamic source convention. Also, the Node/Instance will be appended to the source.

### 5.5.0-librato19

Introducing support for EL7 and Ubuntu Wily.
