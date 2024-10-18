# Pushgateway Prometheus Agent

### How To

- clone to /opt/
  git clone https://git-sysnet.detik.com/sysnet/monitoring/pushgateway.git
- chmod +x /opt/pushgateway/cpu_usage
- chmod +x /opt/pushgateway/memory_usage

### Add to crontab execute every minute
```
#Pushgateway Exporter
* * * * * /opt/pushgateway/cpu_usage
* * * * * /opt/pushgateway/memory_usage
```


### Systemd File

```
vim /usr/lib/systemd/system/pushgateway.service 
```

```sh
[Unit]
Description=Pushgateway Exporter - SystemD by sysAdmin
[Service]
Type=simple
ExecStart=/opt/pushgateway/pushgateway
[Install]
WantedBy=multi-user.target
```
```
systemctl daemon-reload
systemctl enable pushgateway.service
systemctl start pushgateway
systemctl status pushgateway
```

### Supervisord File
```
/etc/supervisor.d/pushgateway.ini
```

```sh
[program:pushgateway]
command=/opt/pushgateway/pushgateway --web.listen-address=:9091
process_name=%(program_name)s
numprocs=1
directory=/opt/pushgateway
umask=022
priority=1
autostart=true
autorestart=true
startsecs=1
startretries=1
exitcodes=0,2
stopsignal=TERM
stopwaitsecs=5
serverurl=AUTO
redirect_stderr=true
stdout_logfile=/var/log/supervisor/pushgateway.log
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
```
