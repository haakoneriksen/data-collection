# Installing Fluentd Using rpm Package

This article explains how to install the td-agent rpm package, the stable Fluentd distribution package maintained by [Treasure Data, Inc](http://www.treasuredata.com/).

NOTE: Currently, Amazon Linux is NOT supported officially because Amazon Linux doesn't guarantee ABI compatibility. RPM for CentOS 7 may work.

## What is td-agent?

[Fluentd](http://www.fluentd.org) is written in Ruby for flexibility, with performance sensitive parts written in C. However, casual users may have difficulty installing and operating a Ruby daemon.

That's why [Treasure Data, Inc](http://www.treasuredata.com/) is providing **the stable distribution of Fluentd**, called `td-agent`. The differences between Fluentd and td-agent can be found <a href="//www.fluentd.org/faqs">here</a>.

NOTE: This installation guide is for td-agent v2, the current stable version. See <a href="td-agent-v1-vs-v2">this page</a> for the comparison between v1 and v2. <a href="install-by-rpm-v1">Here</a> is the rpm installation guide for v1.

## Before Installation

Please follow the [Preinstallation Guide](before-install) to configure your OS properly. This will prevent many unnecessary problems.

## Install from rpm Repository

CentOS and RHEL 5, 6 and 7 are currently supported.

Executing [install-redhat-td-agent2.sh](http://toolbelt.treasuredata.com/sh/install-redhat-td-agent2.sh) will automatically install td-agent on your machine. This shell script registers a new rpm repository at `/etc/yum.repos.d/td.repo` and installs the `td-agent` rpm package.

```bash
$ curl -L http://toolbelt.treasuredata.com/sh/install-redhat-td-agent2.sh | sh
```

NOTE: It's HIGHLY recommended that you set up <b>ntpd</b> on the node to prevent invalid timestamps in your logs.

## Launch Daemon

The `/etc/init.d/td-agent` script is provided to start, stop, or restart the agent.

```bash
$ /etc/init.d/td-agent start
Starting td-agent: [  OK  ]

$ /etc/init.d/td-agent status
td-agent (pid  21678) is running...
```

The following commands are supported:

```bash
$ /etc/init.d/td-agent start
$ /etc/init.d/td-agent stop
$ /etc/init.d/td-agent restart
$ /etc/init.d/td-agent status
```

Please make sure **your configuration file** is located at `/etc/td-agent/td-agent.conf`.

## Post Sample Logs via HTTP

By default, `/etc/td-agent/td-agent.conf` is configured to take logs from HTTP and route them to stdout (`/var/log/td-agent/td-agent.log`). You can post sample log records using the curl command.

```bash
$ curl -X POST -d 'json={"json":"message"}' http://localhost:8888/debug.test
```
