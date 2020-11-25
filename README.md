# [vm1](http://193.40.156.86:10080)  
### ssh
`ubuntu@193.40.156.86 -p10022`
### roles
- agama
- nginx  
- prometheus  
- MySQL-ha
### links
[agama](http://193.40.156.86:10080/)  
[prometheus](http://193.40.156.86:10080/prometheus)   
[MySQL metrics](http://193.40.156.86:10080/mysql-metrics)  
[node metrics](http://193.40.156.86:10080/metrics)   
[Nginx metrics](http://193.40.156.86:10080/nginx-metrics) 

# [vm2](http://193.40.156.86:9280)
### ssh
`ubuntu@193.40.156.86 -p9222`
### roles
- MySQL
- Bind
- Grafana
- Grafana-Docker
### links
[Grafana](http://193.40.156.86:9280/grafana)  
[node metrics](http://193.40.156.86:9280/metrics)  
[MySQL metrics](http://193.40.156.86:9280/mysql-metrics)  
[Bind metrics](http://193.40.156.86:9280/bind-metrics)

# grafana
## CPU load
Metrics: `sum by (instance)(rate(node_cpu{mode!="idle"}[1m]))`
## Memory consumption
Metrics: `node_memory_MemAvailable`

## Bind status
Metrics: `bind_up`
### changing visual (optional)
Panel > Visualization `Stat`
Panel > Display > Calculation `Last` 
Panel > Display > Color mode `Background`
Panel > Display > Graph mode `None`
#### change color green or red (optional)
Field > Threshholds > Base `red`
Add threshhold `green` at 0.5
### show only UP or DOWN (optional)
Field > Value mappings
Mapping type > `Value`
Value > `1`
Text > `UP`
Mapping type > `Value`
Value > `0`
Text > `DOWn`

## DNS queries per minute
Metrics: `rate(bind_resolver_queries_total[1m])`

## MySQL server ID
Metrics: `mysql_global_variables_server_id`
Display > Fields > pick vm

## MySQL status
Metrics: `mysql_up`
(duplicate from Bind status)
Display > Fields > pick vm

## MySQL queries per minute
Metrics: `rate(mysql_global_status_queries[1m])`
Metrics: `rate(mysql_global_status_queries{job="mysql"}[1m])`

## Nginx status
Metrics: `nginx_up`
(duplicate from Bind status)

## Nginx requests per minute
Metrics: `rate(nginx_http_requests_total[1m])`