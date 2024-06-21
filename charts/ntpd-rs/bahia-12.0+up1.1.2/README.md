# ntpd-rs Helm Chart

## Overview

Helm chart for [`ntpd-rs`](https://github.com/pendulum-project/ntpd-rs), an NTP
client to syncronize the clock of Kubernetes hosts.

## Configuration

### Configuration file for `ntpd-rs`

A sample configuration is present in `.Values.config` that configures `ntpd-rs`
to use one server to syncronize the host's clock. You can check the full
reference for this file in
[https://docs.ntpd-rs.pendulum-project.org/man/ntp.toml.5/].
