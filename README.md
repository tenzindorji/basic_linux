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
Login to multiple linux servers:
```
csshx IP IP IP
```

Kill Hung Chef process:
```
sudo ps -ef|awk '/chef/ {print $2}'|xargs -n1 -I% bash -c "sudo kill -9 %"
```

Remove New line from the file: This is very useful for storing secrets and passwords for automation
```
perl -p -i -e 's/\R//g;' filename
```

google Doc: Vlookup:
```
=iferror(VLOOKUP(A64,Sheet33!$A$1:$C$264,3,False),0)
```


AWS:
```
aws --profile cfao-fdsqa --region us-west-2 ec2 describe-network-interfaces
aws --profile cfao-fdsqa --region us-west-2 iam list-server-certificates
```

SSL Management
```
Find End date:
openssl x509 -enddate -noout -in file.pem
Validate cert with secret key:
openssl rsa -in file.pem
Validate cert with end point:
nmap --script ssl-cert fdatafeed-ccwebint-e2e.platform.intuit.net -p 443
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

index=index_name  
| stats dc(tid) as requests by providerId ErrorCode
| eventstats sum(requests) as total_requests sum(eval(if(ErrorCode!="0",requests,null))) as total_failures by providerId
| eval perc_per_provider_and_error=100*(requests/total_requests)
| eval perc_overall_failure_per_provider=100*(total_failures/total_requests)
| where perc_overall_failure_per_provider>=1 and ErrorCode!="0"
| lookup getdata_providerlookup.csv id AS providerId OUTPUT name AS providerName
| fieldformat perc_overall_failure_per_provider=round(perc_overall_failure_per_provider,2)."%"
| fieldformat perc_per_provider_and_error=round(perc_per_provider_and_error,2)."%"
| fields providerId providerName ErrorCode total_requests total_failures requests perc_overall_failure_per_provider perc_per_provider_and_error


index=index_name sourcetype=profile executionDetails AcquireOMSMessageListener providerId = "1234"
| stats count as Total, sum(eval(if(ErrorCode!="0",1,0))) as Failure by providerId
| eval Percentage_Failure=Failure*100/Total
| fieldformat Percentage_Failure=round(Percentage_Failure,1)."%"
| where Percentage_Failure > 0
| lookup getdata_providerlookup.csv id AS providerId OUTPUT name AS providerName
| table providerName, Total, Failure, Percentage_Failure

index=index_name ErrorCode!=0 |timechart span=1m count by ErrorCode
index=index_name ErrorCode=-<errorcode>|chart count by ErrorMessage
index=index_name ErrorCode=7500 |timechart span=15m usenull=f count by ErrorMessage
index=index_name ErrorCode!=0 |timechart span=15m useother=f count by ErrorMessage


Dashboard query:
index=index_name respStatus
| timechart span=$tok_span$  count by respStatus
```


git:

```
#!/bin/bash

# Syncing with upstream

git remote | grep upstream >> /dev/null 2>&1

if [ "$?" != "0" ]; then
        printf "Adding upstream git repo\n"
        git remote add upstream git@github.com:tenzindorji/basic_linux.git
else
        printf "\n"
fi

git pull && \
  git fetch upstream && \
  git merge upstream/master -m "Merge remote-tracking branch 'upstream/master'" && \
  git push

printf "\n"

```
