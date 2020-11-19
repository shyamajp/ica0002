
## Introduction

All services can be restored through Ansible.

All backup data is handled by user 'backup'. To become user "backup" after running next commands:

```bash
sudo su backup
```

## Application related services

### AGAMA -  application

To restore **AGAMA application that uses MySQL database**, run the following command on management host

```bash
ansible-playbook ansible-playbook lab04_web_app.yaml
```

### MySQL - app database

To restore **MySQL and its configuration**, run the following command on management host

```bash
ansible-playbook ansible-playbook lab04_web_app.yaml
ansible-playbook ansible-playbook lab07_grafana.yaml
ansible-playbook ansible-playbook lab10_backups.yaml
```

To restore **MySQL databases and their data**, run the following command on vm2

```bash
duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/
mysql agama < /home/backup/restore/agama.sql

# To check the results run the next command to show AGAMA database contents
mysql -e 'SELECT * FROM agama.item'
```

### uWSGI - app server

To restore **uWSGI**, run the following command on management host

```bash
ansible-playbook ansible-playbook lab04_web_app.yaml
```

## General services

### Ansible - configuration management tool

To restore **Ansible and playbooks**, run the following command on management host

1. Install Ansible if it does not exist.
2. Download Ansible repository into its new location if it does not exist. There are two options:

   1. Download from GitHub
      ```bash
      git@github.com:shyamajp/ica0002.git
      ```
   2. Download from GitLab
      ```bash
      git@gitlab.com:shyamajp/ica0002.git
      ```

### Hosts' users configuration

To restore **juri and roman users** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab02_web_server.yaml
```

### Bind9 - DNS

To restore **Bind9** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab05_dns.yaml
```

#### Hosts' DNS resolvers configuration

To restore **hosts' DNS resolvers configuration** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab05_dns.yaml
```

### Duplicity - backup facility

To restore **Duplicity** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab10_backups.yaml
```

### Cron - time scheduler

To restore **Cron** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab10_backups.yaml
```

### Nginx - web server

To restore **Nginx** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab02_web_server.yaml
```

## Monitoring related services

### InfluxDB - metrics/logs database

To restore **InfluxDB** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab08_logging.yaml
```

To restore **InfluxDB databases and their data**, **do no start with user "backup", start with user "ubuntu"** run the following commands on the host with InfluxDB installed (escalated privileges are required to stop/start InlfuxDB).

```bash
duplicity --no-encryption restore shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/
sudo systemctl stop influxdb
influxd restore -metadir /var/lib/influxdb/meta /home/backup/restore/influxdb
influxd restore -datadir /var/lib/influxdb/data /home/backup/restore/influxdb
sudo systemctl start influxdb

# To check the results run the next command to show databases
influx -execute 'show databases'
```

### Grafana - monitoring visualization

To restore **Grafana** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab07_grafana.yaml
```

To restore **Grafana's configuration**, **do no start with user "backup", start with user "ubuntu"** run the following commands on the host with Grafana installed (escalated privileges are required to stop/start Grafana).

```bash
duplicity --no-encryption restore shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/
sudo systemctl stop prometheus
rsync -a --delete /home/backup/backup/grafana.etc/ /etc/grafana/
rsync -a --delete home/backup/backup/grafana.lib/ /var/lib/grafana/
sudo systemctl start prometheus
```

### Pinger - connectivity/latency checker

To restore **Pinger** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab08_logging.yaml
```

### Prometheus - metrics collector

To restore **Prometheus** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab06_prometheus.yaml
```

To restore **Prometheus databases and their data**, **do no start with user "backup", start with user "ubuntu"** run the following commands on the host with Prometheus installed (escalated privileges are required to stop/start Prometheus).

```bash
duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/
sudo systemctl stop prometheus
rsync -a --delete /home/backup/backup/prometheus/ /var/lib/prometheus/
sudo systemctl start prometheus
```

#### Bind9 exporter

To restore **Bind9 exporter** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab07_grafana.yaml
```

#### MySQL exporter

To restore **MySQL exporter** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab07_grafana.yaml
```

#### Nginx exporters

To restore **Nginx exporters** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab07_grafana.yaml
```

#### Node exporters

To restore **Node exporters** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab06_prometheus.yaml
```

### Rsyslog - syslog facility

To restore **Rsyslog** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab08_logging.yaml
```

### Telegraf - metrics collector

To restore **Telegraf** run the following commands in the Ansible directory containing playbooks on the management host/system (or the system where playbooks were restored):

```bash
ansible-playbook ansible-playbook lab08_logging.yaml
```
