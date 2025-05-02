### **Apache**

**Install**

In order to get statiscics from Apache2 web server, you should enable status module and define status URL. On Debian like system, you can do it by `a2enmod  status` or adding following to Apache main config file: 

```
LoadModule status_module /path/to/apache/mod_status.so
```
After enabling status module you should configure status URL.

```xml
<Location /server-status>
   SetHandler server-status
</Location>
```
Its STRONGLY recommended to limit access to this page ether by limiting by source IP, or enabling authentication on this page : 

```xml
<Location /server-status>
   SetHandler server-status
   Order deny,allow
   Deny from all
   Allow from 127.0.0.1 ::1
   Allow from 192.0.1.0/24
</Location>
```
This will allow access to status page only from Localhost and 192.168.1.0/24 subnet. 

```xml
<Location /server-status>
   SetHandler server-status
   AuthUserFile /etc/apache2/USERLIST
   AuthName "restricted stuff"
   AuthType Basic
   require valid-user
</Location>
```
This will require authentication for status page. 
In real life cluster installation HDFS NameNode doesn't use loopback interface, so make sure that you put right IP of NameNode in config file.  

**Configure**
At Agent side you should make config changes in order to tell Agent how to get statistics from Apache server. 
Config file for Webservers is {AGENT_HOME}/conf/webservers.ini. and it should look like this 

```ini
[Apache]
url: http://127.0.0.1:8080/server-status?auto
user: user
pass: password
auth: True
```

If you have not configured Apache to require login for status page just set `auth: True` and write something as username and password. 
Do not delete user/pass/auth sections, even if you do not use it. 
Symlink or copy  `checks_available/check_apache.py` to `checks_enabled/check_apache.py`. 

**Restart**
```bash
${OE_AGENT_HOME}/puypuy.sh restart
```
**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|apache_busyworkers|Worker processes that servers client requests |gauge|None|
|apache_bytesperreq|Served bytes per request |gauge|Bytes|
|apache_bytespersec|Served bytes per second|rate|Bytes|
|apache_idleworkers|Started workers which do not serve any request|gauge|None|
|apache_reqpersec|Requests per second served by HTTTPD server |rate|OPS|
|apache_totalaccesses|Accesses to server since last restart |counter|None|
|apache_totalkbytes|Bytes served since last restart |counter|kBytes|



### **NginX**


**Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_nginx.py ./
```

**Configure**

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

**Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```
**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|nginx_accept|NginX Accepts per second|rate|OPS|
|nginx_connections|Current connections count |gauge|None|
|nginx_handled|Handled requests per second |rate|OPS|
|nginx_reading|Nginx reading requests |gauge|None|
|nginx_requests|Total requests handled by per second |rate|OPS|
|nginx_waiting|Nginx Waiting for client |counter|None|
|nginx_writing|Nginx writing requests |counter|None|

### **PHP-FPM**

**Install** 

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_phpfpm.py ./
```


**Configure**

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

**Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```


**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|phpfpm_conns_per_sec|FastCGI Connections per second|rate|OPS|
|phpfpm_max_active|Number of maximum active connections |gauge|None|
|phpfpm_max_children|Maximum allowed child processes |gauge|None|
|phpfpm_proc_active|Number of active processes|gauge|None|
|phpfpm_proc_idle|Number of idle processes |gauge|None|
|phpfpm_proc_total|Total number of processes|gauge|None|
|phpfpm_slow_request|Number of slow requests |gauge|OPS|


### **Tomcat**

**Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_tomcat.py ./
```

**Configure**

Apache Tomcat also ships with by default disabled status page. In order to get stats from tomcat, 
you should edit `CATALINA_HOME/conf/tomcat-users.xml` and enable role `manager-jmx`. 
To do this enter following to tomcat-users.xml and restart Tomcat . 

```xml
<role rolename="manager-jmx"/>
<user username="User" password="Pass" roles="manager-jmx"/>
```

If you see 403 Access Denied, try to edit `CATALINA_HOME/webapps/manager/META-INF/context.xml` and somment `Valve` : 

```xml
<Context antiResourceLocking="false" privileged="true" >
<!--
<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
</Context>
```

**Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|tomcat_daemonthreadcount|Amount of running Java daemon threads |gauge|None|
|tomcat_heap_commited|Java Heap committed |gauge|Bytes|
|tomcat_heap_init|Java Heap init|gauge|Bytes|
|tomcat_heap_max|Java Heap max|gauge|Bytes|
|tomcat_heap_used|Java Heap used|gauge|Bytes|
|tomcat_lastgc_0|Young generation GC  time |gauge|Milliseconds |
|tomcat_lastgc_1|Old generation GC  time |gauge|Milliseconds |
|tomcat_nonheap_commited|Java non Heap committed |gauge|Bytes|
|tomcat_nonheap_init|Java non Heap init|gauge|Bytes|
|tomcat_nonheap_max|Java non Heap max|gauge|Bytes|
|tomcat_nonheap_used|Java non Heap used|gauge|Bytes|
|tomcat_peakthreadcount|Peak amount of Java threads |gauge|Bytes|
|tomcat_threadcount|Running threads of tomcat |gauge|None|
|tomcat_totalstartedthreadcount|Total amount of threads started by tomcat |counter|None|

### **Jetty**

**Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_jetty.py ./
```

**Configure**


Jetty comes with pre built support of Jolokia, but module is disabled by default.    
All is needed is just to enable jolokia and restart Jetty.  

```bash
cd $JETTY_HOME
bin/jetty.sh stop
java -jar start.jar --add-to-start=jolokia
bin/jetty.sh start
```

After enabling Jetty-Jolokia copy or symlink `checks_available/check_jetty.py` to `checks_enabled/check_jetty.py`,
Change IP:PORT of Jetty server at section [Jetty] of `conf/webservers.ini`  to match IP:PORT of your server.  

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|jetty_asyncrequests|Number of async requests to Jetty |gauge|None|
|jetty_daemonthreadcount|Number of running daemon threads |gauge|None|
|jetty_dispatchedactive|Number of dispatched threads |gauge|None|
|jetty_{G1,CMS}_old_collectioncount|Number of old gen GC collections |counter|None|
|jetty_{G1,CMS}_old_collectiontime|Time spend for old gen GC|counter|Milliseconds |
|jetty_{G1,CMS}_old_lastgcinfo|Duration in milliseconds of previous old gen GC |gauge|Milliseconds |
|jetty_{G1,CMS}_young_collectioncount|Number of young gen GC collections |counter|None|
|jetty_{G1,CMS}_young_collectiontime|Time spend for young gen GC|counter|Milliseconds |
|jetty_{G1,CMS}_young_lastgcinfo|Duration in milliseconds of previous old gen GC |gauge|Milliseconds |
|jetty_heap_committed|Java heap committed |gauge|Bytes|
|jetty_heap_max|Java heap max|gauge|Bytes|
|jetty_heap_used|Java heap used|gauge|Bytes|
|jetty_nonheap_committed|Java non heap committed |gauge|Bytes|
|jetty_nonheap_max|Java non  heap max|gauge|Bytes|
|jetty_nonheap_used|Java non heap used|gauge|Bytes|
|jetty_peakthreadcount|Peak amount of running threads |gauge|None|
|jetty_requests_rate|Requests per seconds executed on server|rate|OPS|
|jetty_requestsactive|Current active requests |gauge|None|
|jetty_requesttimemean|Mean requests time |gauge|Milliseconds |
|jetty_responses1xx|Responses of type 1xx |counter|None|
|jetty_responses2xx|Responses of type 2xx |counter|None|
|jetty_responses4xx|Responses of type 4xx |counter|None|
|jetty_responses5xx|Responses of type 5xx |counter|None|
|jetty_threadcount|Jetty running threads |gauge|None|



### **Lighttpd**


**Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_lighttpd.py ./
```

**Configure**

Lighttpd is a lightweight  HTTP servers build with performance and security in mind. 
In order to collect metrics from Lighttpd servers you need first to enable and configure stats module.  

Loading module:

```bash
server.modules = ( ..., "mod_status", ... )
```

Status page is disabled by default so you need to enable it

```bash
status.status-url = "/server-status"
```

You can restrict access to status url by allowing access only from certain subnet :

```bash
$HTTP["remoteip"] == "192.168.0.0/24" {
    status.status-url = "/server-status" 
  }

```

Or requesting authentication:
 
```bash
auth.require = ( "/server-status" => ( "realm" ... ) )
```

Please make sure that you configure URL and HOST in `conf/webservers.ini` file.
Default configuration looks like this: 

```ini
[Lighttpd]
address: http://127.0.0.1
stats: /server-status?auto
auth: False
user: netangels
pass: bololo
```

### **HTTP API**

`check_http_api` is basic check for measuring performance of HTTP API servers.
Any HTTP backend can act as monitoring point for this check. 
It will do GET request to your api server and calculate its response time, which will be sent to PuyPuy. 
check_http_api supports HTTP basic authentication. Parameters for this check ate in  HTTP section of `webservers.ini` file: 

```ini
[HTTP]
upstream: http://your.api.com:8080, https://yoursecure.api.com:8443
user: netangels
pass: bololo
auth: True    
```

You can add as many upstreams as you wish to monitor. The only limit is that you must provide full URL of monitored endpoint. 
Final metrics names will look like this : 
```text
http_your_api_com_8080
http_your_secure_api_com_8443
```
    
Where 8080 amd 8443 are ports of configured API server. 
If your api server binds on default HTTP/HTTPS ports, you can exclude port number from `upstream`  of HTTP config:   

```ini
[HTTP]
upstream: http://your.api.com, https://yoursecure.api.com
user: netangels
pass: bololo
auth: True    
```
In this case check names at PuyPuy will look like this:
 
```text
http_your_api_com_80
http_your_secure_api_com_443
```

Please note that `http_` prefix is not describing exact protocol, but just prefixing check name for easy check. 
Thus both HTTP and HTTPS checks will have `http_` prefix.   
