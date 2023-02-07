![Nginx](../../images/nginx.png)

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_nginx.py ./
```

##### **Configure**

Status page os Nginx is disabled by default. You can enable it by adding following to NginX config file and restarting daemon. Code below will do the trick: 

```ini
location /nginx_status {
   stub_status on;
   allow 127.0.0.1;
   deny all;
}
```
We recommend to use separate Server directive status for dedicated port. 

```bash
server {
        listen 127.0.0.1:8088;
        root /var/www;
        index index.html ;
        server_name localhost;
location /nginx_status {
   stub_status on;
}
}
```
This way your NginX server will bind to port 8088 on loop back interface. Its very easy and secure way to provide status. 
After enabling NginX Status, You should add status page parameters to `{AGENT_HOME}/conf/webservers.ini`. 

```ini
[NginX]
address: http://127.0.0.1:8888
stats: /nginx_status
auth: False
user: user
pass: password
```

Change auth enabled from False to True, if you have anabled authentication on  NginX status URL. Please do not delete user/pass/auth, just put some placeholders, if authentication is disabled. 

##### **Restart**

```bash
${OE_AGENT_HOME}/oddeye.sh restart
```
##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|nginx_accept|NginX Accepts per second|rate|OPS|
|nginx_connections|Current connections count |gauge|None|
|nginx_handled|Handled requests per second |rate|OPS|
|nginx_reading|Nginx reading requests |gauge|None|
|nginx_requests|Total requests handled by per second |rate|OPS|
|nginx_waiting|Nginx Waiting for client |counter|None|
|nginx_writing|Nginx writing requests |counter|None|
