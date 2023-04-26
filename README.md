
# Ansible Role:  `node-exporter` 

Ansible role to install and configure [node-exporter](https://github.com/prometheus/node_exporter).

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/bodsch/ansible-node-exporter/main.yml?branch=main)][ci]
[![GitHub issues](https://img.shields.io/github/issues/bodsch/ansible-node-exporter)][issues]
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/bodsch/ansible-node-exporter)][releases]
[![Ansible Quality Score](https://img.shields.io/ansible/quality/50067?label=role%20quality)][quality]

[ci]: https://github.com/bodsch/ansible-node-exporter/actions
[issues]: https://github.com/bodsch/ansible-node-exporter/issues?q=is%3Aopen+is%3Aissue
[releases]: https://github.com/bodsch/ansible-node-exporter/releases
[quality]: https://galaxy.ansible.com/bodsch/node_exporter


If `latest` is set for `node_exporter_version`, the role tries to install the latest release version.  
**Please use this with caution, as incompatibilities between releases may occur!**

The binaries are installed below `/usr/local/bin/node_exporter/${node_exporter_version}` and later linked to `/usr/bin`. 
This should make it possible to downgrade relatively safely.

The application archive is stored on the Ansible controller, unpacked and then the binaries are copied to the target system.
The cache directory can be defined via the environment variable `CUSTOM_LOCAL_TMP_DIRECTORY`. 
By default it is `${HOME}/.cache/ansible/node_exporter`.
If this type of installation is not desired, the download can take place directly on the target system. 
However, this must be explicitly activated by setting `node_exporter_direct_download` to `true`.

## Requirements & Dependencies

Ansible Collections

- [bodsch.core](https://github.com/bodsch/ansible-collection-core)
- [bodsch.scm](https://github.com/bodsch/ansible-collection-scm)

```bash
ansible-galaxy collection install bodsch.core
ansible-galaxy collection install bodsch.scm
```
or
```bash
ansible-galaxy collection install --requirements-file collections.yml
```


## Operating systems

Tested on

* Arch Linux
* Debian based
    - Debian 10 / 11
    - Ubuntu 20.10
* RedHat based
    - CentOS 8 (**not longer supported**)
    - Alma Linux 8
    - Rocky Linux 8
    - Oracle Linux 8

## Contribution

Please read [Contribution](CONTRIBUTING.md)

## Development,  Branches (Git Tags)

The `master` Branch is my *Working Horse* includes the "latest, hot shit" and can be complete broken!

If you want to use something stable, please use a [Tagged Version](https://github.com/bodsch/node_exporter/tags)!

## Configuration

```yaml
node_exporter_version: "1.3.1"
node_exporter_release_download_url: https://github.com/prometheus/node_exporter/releases

node_exporter_system_user: node_exporter
node_exporter_system_group: node_exporter
node_exporter_config_dir: /etc/node_exporter
node_exporter_textfile_dir: /var/lib/node_exporter

node_exporter_direct_download: false

node_exporter_service:
  log:
    level: warn
  web:
    listen_address: "0.0.0.0:9100"
    telemetry_path: "/metrics"

node_exporter_tls_server: {}

node_exporter_http_server: {}

node_exporter_basic_auth_users: {}

node_exporter_collectors:
  enabled:
    - textfile:
        directory: "/var/lib/node_exporter"
  disabled: []
```

### example

see [molecule/default/group_vars/all/vars.yml](molecule/default/group_vars/all/vars.yml)

### collectors

| collector name | description |
| :---           | :----        |
| `bcache.priorityStats`              | Expose expensive priority stats. |
| `cpu.guest`                         | Enables metric node_cpu_guest_seconds_total |
| `cpu.info`                          | Enables metric cpu_info |
| `cpu.info.flags-include`            | Filter the `flags` field in cpuInfo with a value that must be a regular expression | 
| `cpu.info.bugs-include`             | Filter the `bugs` field in cpuInfo with a value that must be a regular expression |
| `diskstats.ignored-devices=`        | Regexp of devices to ignore for diskstats |
| `ethtool.device-include=`           | Regexp of ethtool devices to include (mutually exclusive to device-exclude) |
| `ethtool.device-exclude=`           | Regexp of ethtool devices to exclude (mutually exclusive to device-include) |
| `ethtool.metrics-include=`          | egexp of ethtool stats to include |
| `filesystem.mount-points-exclude=`  | Regexp of mount points to exclude for filesystem collector |
| `filesystem.fs-types-exclude=`      | Regexp of filesystem types to exclude for filesystem collector |
| `ipvs.backend-labels=`              | Comma separated list for IPVS backend stats labels |
| `netclass.ignored-devices=`         | Regexp of net devices to ignore for netclass collector |
| `netclass.ignore-invalid-speed`     | Ignore devices where the speed is invalid. This will be the default behavior in 2.x |
| `netdev.device-include=`            | Ignore devices where the speed is invalid. This will be the default behavior in 2.x |
| `netdev.device-exclude=`            | Regexp of net devices to exclude (mutually exclusive to device-include) |
| `netdev.address-info`               | Collect address-info for every device |
| `netstat.fields=`                   | Regexp of fields to return for netstat collector |
| `ntp.server="127.0.0.1"`            | NTP server to use for ntp collector |
| `ntp.protocol-version=4`            | NTP protocol version |
| `ntp.server-is-local`               | Certify that `collector.ntp.server` address is not a public ntp server |
| `ntp.ip-ttl=1`                      | IP TTL to use while sending NTP query |
| `ntp.max-distance=3.46608s`         | Max accumulated distance to the root |
| `ntp.local-offset-tolerance=1ms`    | Offset between local clock and local ntpd time to tolerate |
| `perf.cpus=""`                      | List of CPUs from which perf metrics should be |
| `perf.tracepoint=`                  | perf tracepoint that should be collected |
| `powersupply.ignored-supplies=""`   | Regexp of power supplies to ignore for powersupplyclass collector |
| `qdisc.fixtures=""`                 | test fixtures to use for qdisc collector end-to-end testing |
| `runit.servicedir=""`               | Path to runit service directory |
| `supervisord.url=""`                | XML RPC endpoint |
| `systemd.unit-include="`            | Regexp of systemd units to include. Units must both match include and not match exclude to be included |
| `systemd.unit-exclude="`            | Regexp of systemd units to exclude. Units must both match include and not match exclude to be included |
| `systemd.enable-task-metrics`       | Enables service unit tasks metrics `unit_tasks_current` and `unit_tasks_max` |
| `systemd.enable-restarts-metrics`   | Enables service unit metric `service_restart_total` |
| `systemd.enable-start-time-metrics` | Enables service unit metric `unit_start_time_seconds` |
| `tapestats.ignored-devices="`       | Regexp of devices to ignore for tapestats |
| `textfile.directory=""`             | Directory to read text files with metrics from |
| `vmstat.fields="`                   | Regexp of fields to return for vmstat collector |
| `wifi.fixtures=""`                  | test fixtures to use for wifi collector metrics           |
| `arp`                               | Enable the arp collector (default: *enabled*)             |
| `bcache`                            | Enable the bcache collector (default: *enabled*)          |
| `bonding`                           | Enable the bonding collector (default: *enabled*)         |
| `btrfs`                             | Enable the btrfs collector (default: *enabled*)           |
| `buddyinfo`                         | Enable the buddyinfo collector (default: *disabled*)      |
| `conntrack`                         | Enable the conntrack collector (default: *enabled*)       |
| `cpu`                               | Enable the cpu collector (default: *enabled*)             |
| `cpufreq`                           | Enable the cpufreq collector (default: *enabled*)         |
| `diskstats`                         | Enable the diskstats collector (default: *enabled*)       |
| `dmi`                               | Enable the dmi collector (default: *enabled*)             |
| `drbd`                              | Enable the drbd collector (default: *disabled*)           |
| `drm`                               | Enable the drm collector (default: *disabled*)            |
| `edac`                              | Enable the edac collector (default: *enabled*)            |
| `entropy`                           | Enable the entropy collector (default: *enabled*)         |
| `ethtool`                           | Enable the ethtool collector (default: *disabled*)        |
| `fibrechannel`                      | Enable the fibrechannel collector (default: *enabled*)    |
| `filefd`                            | Enable the filefd collector (default: *enabled*)          |
| `filesystem`                        | Enable the filesystem collector (default: *enabled*)      |
| `hwmon`                             | Enable the hwmon collector (default: *enabled*)           |
| `infiniband`                        | Enable the infiniband collector (default: *enabled*)      |
| `interrupts`                        | Enable the interrupts collector (default: *disabled*)     |
| `ipvs`                              | Enable the ipvs collector (default: *enabled*)            |
| `ksmd`                              | Enable the ksmd collector (default: *disabled*)           |
| `lnstat`                            | Enable the lnstat collector (default: *disabled*)         |
| `loadavg`                           | Enable the loadavg collector (default: *enabled*)         |
| `logind`                            | Enable the logind collector (default: *disabled*)         |
| `mdadm`                             | Enable the mdadm collector (default: *enabled*)           |
| `meminfo`                           | Enable the meminfo collector (default: *enabled*)         |
| `meminfo_numa`                      | Enable the meminfo_numa collector (default: *disabled*)   |
| `mountstats`                        | Enable the mountstats collector (default: *disabled*)     |
| `netclass`                          | Enable the netclass collector (default: *enabled*)        |
| `netdev`                            | Enable the netdev collector (default: *enabled*)          |
| `netstat`                           | Enable the netstat collector (default: *enabled*)         |
| `network_route`                     | Enable the network_route collector (default: *disabled*)  |
| `nfs`                               | Enable the nfs collector (default: *enabled*)             |
| `nfsd`                              | Enable the nfsd collector (default: *enabled*)            |
| `ntp`                               | Enable the ntp collector (default: *disabled*)            |
| `nvme`                              | Enable the nvme collector (default: *enabled*)            |
| `os`                                | Enable the os collector (default: *enabled*)              |
| `perf`                              | Enable the perf collector (default: *disabled*)           |
| `powersupplyclass`                  | Enable the powersupplyclass collector (default: *enabled*)|
| `pressure`                          | Enable the pressure collector (default: *enabled*)        |
| `processes`                         | Enable the processes collector (default: *disabled*)      |
| `qdisc`                             | Enable the qdisc collector (default: *disabled*)          |
| `rapl`                              | Enable the rapl collector (default: *enabled*)            |
| `runit`                             | Enable the runit collector (default: *disabled*)          |
| `schedstat`                         | Enable the schedstat collector (default: *enabled*)       |
| `sockstat`                          | Enable the sockstat collector (default: *enabled*)        |
| `softnet`                           | Enable the softnet collector (default: *enabled*)         |
| `stat`                              | Enable the stat collector (default: *enabled*)            |
| `supervisord`                       | Enable the supervisord collector (default: *disabled*)    |
| `systemd`                           | Enable the systemd collector (default: *disabled*)        |
| `tapestats`                         | Enable the tapestats collector (default: *enabled*)       |
| `tcpstat`                           | Enable the tcpstat collector (default: *disabled*)        |
| `textfile`                          | Enable the textfile collector (default: *enabled*)        |
| `thermal_zone`                      | Enable the thermal_zone collector (default: *enabled*)    |
| `time`                              | Enable the time collector (default: *enabled*)            |
| `timex`                             | Enable the timex collector (default: *enabled*)           |
| `udp_queues`                        | Enable the udp_queues collector (default: *enabled*)      |
| `uname`                             | Enable the uname collector (default: *enabled*)           |
| `vmstat`                            | Enable the vmstat collector (default: *enabled*)          |
| `wifi`                              | Enable the wifi collector (default: *disabled*)           |
| `xfs`                               | Enable the xfs collector (default: *enabled*)             |
| `zfs`                               | Enable the zfs collector (default: *enabled*)             |
| `zoneinfo`                          | Enable the zoneinfo collector (default: *disabled*)       |


---

## Author and License

- Bodo Schulz

## License

[Apache](LICENSE)

**FREE SOFTWARE, HELL YEAH!**
