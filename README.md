#zbx_nginx_template

Zabbix template for Nginx (python)

It's accumulate nginx stats and pars diff of access.log and push result in Zabbix throuhg trap-messages

##System requirements

- [python](http://www.python.org/downloads/)
- [nginx](http://nginx.org/) with configured http_stub_status_module and access.log

## What's logging:

- Request\sec
- Keepalive connections
- Active connections
- Header and body reading
- Accepted, handled connections

## Install

1) Put `zbx_nginx_stats.py` into your scripts path (like: `/etc/zabbix/script/nginx/`).

2) Change next section in zbx_nginx_stats.py, to your configuration:

```
zabbix_host = '127.0.0.1'   # Zabbix server IP
zabbix_port = 10051         # Zabbix server port
hostname = 'Zabbix server'  # Name of monitored server, like it shows in zabbix web ui
time_delta = 1              # grep interval in minutes

# URL to nginx stat (http_stub_status_module)
stat_url = 'https://nginx.server/nginx_stat'

# Nginx log file path
nginx_log_file_path = '/var/log/nginx/access.log'

# Optional Basic Auth
username = 'user'
password = 'pass'

# Temp file, with log file cursor position
seek_file = '/tmp/nginx_log_stat'
```

3) In script path (`/etc/zabbix/script/nginx/`) do:
```
chmod +x zbx_nginx_stats.py
```

4) Configure cron to run script every one minute:
```
$ sudo crontab -e

*/1 * * * * /etc/zabbix/script/nginx/zbx_redis_module.py
```

5) Import `zbx_nginx_template.xml` into zabbix in Tepmplate section web gui.

That is all :)


