
# Introduction

All services can be restored through Ansible.

All backup data is handled by user 'backup'. To become user "backup" after running next commands:

```bash
sudo su backup
```

# Restoration process
Please run below depending on your need!

## APPLICATION SERVICES

### AGAMA
run on management host

```bash
ansible-playbook ansible-playbook lab04_web_app.yaml
```

### MySQL
Run on management host
```bash
ansible-playbook ansible-playbook lab04_web_app.yaml
```

### MySQL database
run on vm2 as ubuntu

```bash
duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/
mysql agama < /home/backup/restore/agama.sql

# To check the results run the next command to show AGAMA database contents
mysql -e 'SELECT * FROM agama.item'
```

### uWSGI

run on management host

```bash
ansible-playbook ansible-playbook lab04_web_app.yaml
```


### Nginx
Run on management host
```bash
ansible-playbook ansible-playbook lab02_web_server.yaml
```

## MANAGEMENT
### Ansible
Run on management host
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

### Users
Run on management host
```bash
ansible-playbook ansible-playbook lab02_web_server.yaml
```


## LOGGING AND MONITORING SERVICES
### Node exporters/Prometheus
Run on management host
```bash
ansible-playbook ansible-playbook lab06_prometheus.yaml
```

### Prometheus database
Run on vm1 as backup
```bash
duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/
sudo systemctl stop prometheus
rsync -a --delete /home/backup/backup/prometheus/ /var/lib/prometheus/
sudo systemctl start prometheus
```

### Nginx exporters/MySQL exporters/Bind9 exporter/Grafana
Run on management host
```bash
ansible-playbook ansible-playbook lab07_grafana.yaml
```

### Grafana configuration
Run on vm2 as ubuntu
```bash
duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/
sudo systemctl stop prometheus
rsync -a --delete /home/backup/backup/grafana.etc/ /etc/grafana/
rsync -a --delete home/backup/backup/grafana.lib/ /var/lib/grafana/
sudo systemctl start prometheus
```

### Telegraf/Rsyslog/pinger/InfluxDB
Run on management host
```bash
ansible-playbook ansible-playbook lab08_logging.yaml
```

### InfluxDB database
Run on vm2 as ubuntu

```bash
duplicity --no-encryption restore shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/
sudo systemctl stop influxdb
influxd restore -metadir /var/lib/influxdb/meta /home/backup/restore/influxdb
influxd restore -datadir /var/lib/influxdb/data /home/backup/restore/influxdb
sudo systemctl start influxdb

# To check the results run the next command to show databases
influx -execute 'show databases'
```


## BACKUP SERVICES

### Duplicity
Run on management host
```bash
ansible-playbook ansible-playbook lab10_backups.yaml
```

### Cron
Run on management host

```bash
ansible-playbook ansible-playbook lab10_backups.yaml
```


## DNS
### Bind9
Run on management host
```bash
ansible-playbook ansible-playbook lab05_dns.yaml
```

#### Hosts' DNS resolvers configuration
Run on management host
```bash
ansible-playbook ansible-playbook lab05_dns.yaml
```

