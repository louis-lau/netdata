<!--startmeta
custom_edit_url: "https://github.com/netdata/netdata/edit/master/collectors/charts.d.plugin/libreswan/README.md"
meta_yaml: "https://github.com/netdata/netdata/edit/master/collectors/charts.d.plugin/libreswan/metadata.yaml"
sidebar_label: "Libreswan"
learn_status: "Published"
learn_rel_path: "Data Collection/VPNs"
most_popular: False
message: "DO NOT EDIT THIS FILE DIRECTLY, IT IS GENERATED BY THE COLLECTOR'S metadata.yaml FILE"
endmeta-->

# Libreswan


<img src="https://netdata.cloud/img/libreswan.png" width="150"/>


Plugin: charts.d.plugin
Module: libreswan

<img src="https://img.shields.io/badge/maintained%20by-Netdata-%2300ab44" />

## Overview

Monitor Libreswan performance for optimal IPsec VPN operations. Improve your VPN operations with Netdata''s real-time metrics and built-in alerts.

The collector uses the `ipsec` command to collect the information it needs.

This collector is supported on all platforms.

This collector supports collecting metrics from multiple instances of this integration, including remote instances.


### Default Behavior

#### Auto-Detection

This integration doesn't support auto-detection.

#### Limits

The default configuration for this integration does not impose any limits on data collection.

#### Performance Impact

The default configuration for this integration is not expected to impose a significant performance impact on the system.


## Metrics

Metrics grouped by *scope*.

The scope defines the instance that the metric belongs to. An instance is uniquely identified by a set of labels.



### Per IPSEC tunnel

Metrics related to IPSEC tunnels. Each tunnel provides its own set of the following metrics.

This scope has no labels.

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| libreswan.net | in, out | kilobits/s |
| libreswan.uptime | uptime | seconds |



## Alerts

There are no alerts configured by default for this integration.


## Setup

### Prerequisites

#### Install charts.d plugin

If [using our official native DEB/RPM packages](https://github.com/netdata/netdata/blob/master/packaging/installer/UPDATE.md#determine-which-installation-method-you-used), make sure `netdata-plugin-chartsd` is installed.


#### Permissions to execute `ipsec`

The plugin executes 2 commands to collect all the information it needs:

```sh
ipsec whack --status
ipsec whack --trafficstatus
```

The first command is used to extract the currently established tunnels, their IDs and their names.
The second command is used to extract the current uptime and traffic.

Most probably user `netdata` will not be able to query libreswan, so the `ipsec` commands will be denied.
The plugin attempts to run `ipsec` as `sudo ipsec ...`, to get access to libreswan statistics.

To allow user `netdata` execute `sudo ipsec ...`, create the file `/etc/sudoers.d/netdata` with this content:

```
netdata ALL = (root) NOPASSWD: /sbin/ipsec whack --status
netdata ALL = (root) NOPASSWD: /sbin/ipsec whack --trafficstatus
```

Make sure the path `/sbin/ipsec` matches your setup (execute `which ipsec` to find the right path).



### Configuration

#### File

The configuration file name for this integration is `charts.d/libreswan.conf`.


You can edit the configuration file using the `edit-config` script from the
Netdata [config directory](https://github.com/netdata/netdata/blob/master/docs/configure/nodes.md#the-netdata-config-directory).

```bash
cd /etc/netdata 2>/dev/null || cd /opt/netdata/etc/netdata
sudo ./edit-config charts.d/libreswan.conf
```
#### Options

The config file is sourced by the charts.d plugin. It's a standard bash file.

The following collapsed table contains all the options that can be configured for the libreswan collector.


<details><summary>Config options</summary>

| Name | Description | Default | Required |
|:----|:-----------|:-------|:--------:|
| libreswan_update_every | The data collection frequency. If unset, will inherit the netdata update frequency. | 1 | False |
| libreswan_priority | The charts priority on the dashboard | 90000 | False |
| libreswan_retries | The number of retries to do in case of failure before disabling the collector. | 10 | False |
| libreswan_sudo | Whether to run `ipsec` with `sudo` or not. | 1 | False |

</details>

#### Examples

##### Run `ipsec` without sudo

Run the `ipsec` utility without sudo

```yaml
# the data collection frequency
# if unset, will inherit the netdata update frequency
#libreswan_update_every=1

# the charts priority on the dashboard
#libreswan_priority=90000

# the number of retries to do in case of failure
# before disabling the module
#libreswan_retries=10

# set to 1, to run ipsec with sudo (the default)
# set to 0, to run ipsec without sudo
libreswan_sudo=0

```


## Troubleshooting

### Debug Mode

To troubleshoot issues with the `libreswan` collector, run the `charts.d.plugin` with the debug option enabled. The output
should give you clues as to why the collector isn't working.

- Navigate to the `plugins.d` directory, usually at `/usr/libexec/netdata/plugins.d/`. If that's not the case on
  your system, open `netdata.conf` and look for the `plugins` setting under `[directories]`.

  ```bash
  cd /usr/libexec/netdata/plugins.d/
  ```

- Switch to the `netdata` user.

  ```bash
  sudo -u netdata -s
  ```

- Run the `charts.d.plugin` to debug the collector:

  ```bash
  ./charts.d.plugin debug 1 libreswan
  ```

