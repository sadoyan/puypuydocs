![PHP](../../images/php.png)

##### **Install** 

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_phpfpm.py ./
```


##### **Configure**

PHP-FPM provides very usefull statistics about its internal processes. To enable stats for php-fpm edit php.ini file and add following 
`pm.status_path = /fpm-status`.
FPM status page should be configured at webserver as well. Asuming you are using NginX. So cinfig will look like this: 

```text
location /fpm-status {
    fastcgi_pass   unix:/var/run/php5-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
    allow 127.0.0.1;
    deny all;
}
```
In conjunction with NginX config above, full config file for NginX+FPM status will look like this : 

```text
server {
        listen 127.0.0.1:8088;
        root /var/www;
        index index.html ;
        server_name localhost;
location /nginx_status {
   stub_status on;
}
location /fpm-status {
    fastcgi_pass   unix:/var/run/php5-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}
}
```
This is easy way to keep stats related configs in the same place and keep tracking of configuration files. 
After PHP-FPM is configured you can copy or symlink `checks_available/check_phpfpm.py` to `checks_enabled/check_phpfpm.py`, configure Agent in conf/webservers.ini and restart Agent daemon.

```ini
[PhpFPM]
address: http://127.0.0.1:8888
stats: /fpm-status
auth: False
user: User
pass: Pass
```

##### **Restart**

```bash
${OE_AGENT_HOME}/oddeye.sh restart
```


##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|phpfpm_conns_per_sec|FastCGI Connections per second|rate|OPS|
|phpfpm_max_active|Number of maximum active connections |gauge|None|
|phpfpm_max_children|Maximum allowed child processes |gauge|None|
|phpfpm_proc_active|Number of active processes|gauge|None|
|phpfpm_proc_idle|Number of idle processes |gauge|None|
|phpfpm_proc_total|Total number of processes|gauge|None|
|phpfpm_slow_request|Number of slow requests |gauge|OPS|
