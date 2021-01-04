# Infrastructure
Each aplication stack machine should have these services set up and running: 
## Haproxy and exporter
- prior     : vm1
- inferior  : vm2

## Bind and exporter
- master    : vm3
- slaves    : vm1, vm2

## Agama in Docker conitaner
- vm1, vm2

## MySQL and exporter
- master    : vm2
- slave     : vm1

## InfluxDB
- vm3

## Telefraf
- vm3

## Prometheus
- vm3

## Grafana
- vm3
