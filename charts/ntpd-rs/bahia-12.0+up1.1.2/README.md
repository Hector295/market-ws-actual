# ntpd-rs Helm Chart

## Overview

Helm chart for [`ntpd-rs`](https://github.com/pendulum-project/ntpd-rs), an NTP
client to syncronize the clock of Kubernetes hosts.

## Prerequisites

Before you install this chart verify that there aren't any NTP clients running
on the hosts like `chrony` or `systemd-timesyncd`. To stop and disable
`systemd-timesyncd` run:

```shell
systemctl stop systemd-timesyncd
systemctl disable systemd-timesyncd
```

## Configuration

### Configuration file for `ntpd-rs`

A sample configuration is present in `.Values.config` that configures `ntpd-rs`
to use one server to syncronize the host's clock. You can check the full
reference for this file
[here](https://docs.ntpd-rs.pendulum-project.org/man/ntp.toml.5/).
