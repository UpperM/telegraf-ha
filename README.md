<p align="center">

  <h1 align="center">Telegraf - High Avaibility</h1>
 </p>

1. [Description](#description)
2. [Installation](#installation)
    1. [Setup](#setup)
    2. [Configuration](#configuration)
    3. [Run](#run)
3. [Usage](#usage)

## Description

This project adds a basic fail over mode to Telegraf. It's useful when you use Telegraf in pull mode.<br>

Slave server check master status (reachable and telegraf service status) each 60s (default interval) and sync telegraf config of master.<br>
If the master is not reachable, or telegraf service is down, the slave start his telegraf-slave instance with the configuration synced.<br>
When master come back, salve server stop telegraf-slave service.<br>

A copy of telegraf service is created (telegraf-salve.service), so you can run your telegraf for monitoring your slave server.<br>
## Tested on

- Debian Buster
- [Telegraf](https://github.com/influxdata/telegraf)

Other versions will probably work but are untested.

## Installation
### Setup

Download telegraf-ha.deb

```sh
cd /tmp
wget https://github.com/UpperM/telegraf-ha/releases/latest/download/telegraf-ha.deb
```
Install package
```sh
dpkg -i telegraf-ha.deb
```
Reload systemd
```sh
systemctl daemon-reload
```
### Configuration
The configuration file is stored in /etc/telegraf-ha/telegraf-ha.conf

```sh
# Hostname or IP of master server
MASTER_HOSTNAME=srv-telegraf1

# Private key used for ssh connection to master
AUTH_PRIVATE_KEY="~/.ssh/id_rsa"

# User used for ssh connection to master
AUTH_USER=root

# Folder where are stored config in master
CONF_PATH_REMOTE="/etc/telegraf"

# Local folder where are stored config from master
CONF_PATH_SYNC="/etc/telegraf-ha"

# Interval to check master (in second)
CHECK_INTERVAL=60

# Path where are sotred the log
LOG_FILE="/var/log/telegraf-ha.log"

# Enable debug mode
DEBUG=TRUE
```

### Run
Start telegraf-ha service
```sh
systemctl start telegraf-ha
```

You can see the log while process is running
```sh
tail -f /var/log/telegraf-ha.log
```
