# basic_linux
Find Kernal version:
```
uname -a
```
Find current IP address
Old way:
```
ifconfig
```
New way:
```
ip
```
How to check free disk space:
```
df -ah
```
How do you manage services on a system?
Newer system
```
systemd
```
Older system:
```
service <service_name> stop/start/status/restart
```

Remove New line:
```
perl -p -i -e 's/\R//g;' filename
```
Vlookup:
```
=iferror(VLOOKUP(A64,Sheet33!$A$1:$C$264,3,False),0)
```
Kill Hung Chef process:
```
sudo ps -ef|awk '/chef/ {print $2}'|xargs -n1 -I% bash -c "sudo kill -9 %"
```

Validate SSL certificate:
```
nmap --script ssl-cert fdatafeed-ccwebint-e2e.platform.intuit.net -p 443
```

AWS:
```
aws --profile cfao-fdsqa --region us-west-2 ec2 describe-network-interfaces
aws --profile cfao-fdsqa --region us-west-2 iam list-server-certificates
```

csshx IP IP IP opens multiple linux box

SSL END DATE:
```
openssl x509 -enddate -noout -in file.pem
```

Splunk Queries:

```
index=index_name|top respStatus
index=index_name earliest=-15m latest=now|stats avg(resp_time_total) as avg_resp| search avg_resp > 2000
index=index_name sourcetype=tomcat|where http_responsecode> 199 |stats count as throughput
index=index_name errorCode!=0 |timechart span=5m count by errorCode
index=index_name total_request_execution_time sourctype=webservice|dedup tid|stats count by error_code
index=index_name |stats dc(tid) as total dc(eval(if(status>=200 AND status<=299, tid, null))) as success dc(eval(if(status>=399, tid, null))) as failure|eval successrate=100*(success/total)
index=index_name Xhost=myhost.com |table _time,xHost,status,svcTime
index=index_name TotalResponseTime|bucket _time span=5m |stats perc50(TotalResponseTime) as Avg_resp_time perc99(TotalResponseTime) as percentile_99
```
