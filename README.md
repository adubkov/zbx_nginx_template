#zbx_nginx_template

Zabbix template for Nginx (python)

It's accumulate nginx stats and parse the access.log (just pice of log at once) and push result in Zabbix through trap-messages

##System requirements

- [python](http://www.python.org/downloads/)
- [nginx](http://nginx.org/) with configured http_stub_status_module and access.log

## What's logging:

- Request\sec
- Response codes (200,301,302,403,404,500,503)\min
- Active\Keepalive connections
- Header and body reading
- Accepted, handled connections

## Install

1) Put `zbx_nginx_stats.py` into your scripts path (like: `/etc/zabbix/script/nginx/`) on your Zabbix agent hosts.

2) Change next section in zbx_nginx_stats.py, to your configuration:

```
zabbix_host = '127.0.0.1'   # Zabbix server IP
zabbix_port = 10051         # Zabbix server port
hostname = 'Zabbix Agent'   # Name of monitored host, like it shows in zabbix web ui
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

*/1 * * * * /etc/zabbix/script/nginx/zbx_nginx_stats.py
```

5) Import `zbx_nginx_template.xml` into zabbix in Tepmplate section web gui.

6) Add the following configurations to you Nginx configuration file.
```
location /nginx_stat {
  stub_status on;       # Turn on nginx stats
  access_log   off;     # We do not need logs for stats
  allow 127.0.0.1;      # Security: Only allow access from IP
  deny all;             # Deny requests from the other of the world
}
```

That is all :)


