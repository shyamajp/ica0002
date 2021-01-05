# Introduction

All services can be restored through Ansible.
Exporters are restored together with its services.

# Ansible (management host)
## Data Restoration
Clone from GitHub
```bash
git@github.com:shyamajp/ica0002.git
```

# Nginx (all)
## Service Recovery
Fix service
```bash
ansible-playbook infra.yaml -t nginx
```

# Node exporters (all)
## Service Recovery
Fix service
```bash
ansible-playbook infra.yaml -t node
```

# Reverse Proxy (all)
## Service Recovery
Fix service
```bash
ansible-playbook infra.yaml -t reverse_proxy
```

# Agama(all)
## Service Recovery
Fix service
```bash
ansible-playbook infra.yaml -t docker,agama
```

# Reverse Proxy (all)
## Service Recovery
Fix service
```bash
ansible-playbook infra.yaml -t reverse_proxy
```

# MySQL (Slave - vm1, Master - vm2)
## Service Recovery
1. Fix service
Run on management host
```bash
ansible-playbook infra.yaml -t mysql
```

## Data Restoration
** Slave (vm1) **
2.  Fetch data from master
Run on Slave (vm1) as root
```bash
mysql
STOP SLAVE;
CHANGE MASTER TO MASTER_HOST='vm2',MASTER_USER='replication',MASTER_PASSWORD='<mysql-replication-password>';
RESET SLAVE;
START SLAVE;
SHOW SLAVE STATUS\G
```

**MASTER (vm2)**
### From machine
2. Restore data
Run on Master (vm2) as root
```bash
systemctl stop mysql
mysql agama < /home/backup/backup/mysql/agama.sql
systemctl start mysql
```

> If it does not work, try step 4 and 5.

### From backup server
3. Fetch data from backup server
Run on Master (vm2) as backup
```bash
duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/ 
```

4. Restore data from backup server
Run on Master (vm2) as root
```bash
mysql agama < /home/backup/restore/mysql/agama.sql
systemctl start mysql
```

# Prometheus (vm3)
## Service Recovery
1. Fix service
Run on management host
```bash
ansible-playbook infra.yaml -t prometheus
```

# Grafana (vm3)

## Service Recovery
1. Fix service
Run on management host
```bash
ansible-playbook infra.yaml -t grafana
```

## Data Restoration
### From machine
2. Restore data from machine
Run on Grafana (vm3) as root
```bash
cp -a /home/backup/backup/grafana/grafana/* /opt/docker/grafana/
chown -R 472:472 /opt/docker/grafana/
docker start grafana
```

> If this won't work, you can also restore from backup server. Check step 3.

### From backup server
3. Fetch data from backup server
Run on Grafana (vm3) as backup
```bash
duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/ 
```

4. Restore data from backup server
Run on Grafana (vm3) as root
```bash
cp -a /home/backup/restore/grafana/grafana/* /opt/docker/grafana/
chown -R 472:472 /opt/docker/grafana/
docker start grafana
```

> If this won't work, you can also restore manually. Check step 5.

### Manually
5. Go to Grafana page on Grafana (vm3)

6. Add data sources
Go to Configuration > Data Sources > Add Data Source
- Prometheus
URL: http://prometheus.premiumfriday.ttu:9090/prometheus

- InfluxDB/syslog
URL: http://influxdb.premiumfriday.ttu:8086
Database: telegraf

- InfluxDB/latency
URL: http://influxdb.premiumfriday.ttu:8086
Database: latency

7. Import dashboard
Go to Create > Import, copy and paste grafana_dashboard.json from this repository.


# InfluxDB (vm3)
## Service Recovery
1. Fix service
Run on management host
```bash
ansible-playbook infra.yaml -t influxdb
```

## Data Restoration (Optional)
### From machine
2. Restore data
Run on InfluxDB (vm3) as root
```bash
systemctl stop influxd
influxd restore -metadir /var/lib/influxdb/meta /home/backup/backup/influxd
influxd restore -datadir /var/lib/influxdb/dara /home/backup/backup/influxd
systemctl start influxd
```

### From backup server
1. Fetch data
Run on InfluxDB (vm3) as backup
```bash
duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/ 
```

2. Restore data
Run on InfluxDB (vm3) as root
```bash
systemctl stop influxd
influxd restore -metadir /var/lib/influxdb/meta /home/restore/influxd
influxd restore -datadir /var/lib/influxdb/dara /home/restore/influxd
systemctl start influxd
```

# Pinger (vm3)
## Service Recovery
1. Fix service
Run on management host
```bash
ansible-playbook infra.yaml -t pinger
```

## Data Restoration (Optional)
Check InfluxDB Data Restoration

# Rsyslog (vm3)
## Service Recovery
1. Fix service
Run on management host
```bash
ansible-playbook infra.yaml -t rsyslog
```

## Data Restoration (Optional)
Check InfluxDB Data Restoration

> If it does not work, try step 3 and 4.

3. Fetch data from backup server
Run on InfluxDB (vm3) as backup
```bash
backup  duplicity --no-encryption restore rsync://shyamajp@backup.premiumfriday.ttu//home/shyamajp/ /home/backup/restore/ 
```

4. Restore data from backup server
Run on InfluxDB (vm3) as root
```bash
sudo systemctl stop influxdb
influxd restore -metadir /var/lib/influxdb/meta /home/backup/restore/influxdb
influxd restore -datadir /var/lib/influxdb/data /home/backup/restore/influxdb
sudo systemctl start influxdb
```

# DNS/Bind9 (Slaves - vm1, vm2, Master - vm3)
## Service Recovery
Fix service
```bash
ansible-playbook infra.yaml -t bind,resolve
```

# HA/HAProxy/Keepalived (vm1, vm2)
## Service Recovery
Fix service
```bash
ansible-playbook infra.yaml -t haproxy
```

# Backup/Duplicity/Cron (all)
## Service Recovery
Fix service
```bash
ansible-playbook infra.yaml -t backup
```
