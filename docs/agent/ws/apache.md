![Apache](../../images/httpd.png)

##### **Install**

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

##### **Configure**
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

##### **Restart**
```bash
${PUYPUY_HOME}/puypuy.sh restart
```
##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|apache_busyworkers|Worker processes that servers client requests |gauge|None|
|apache_bytesperreq|Served bytes per request |gauge|Bytes|
|apache_bytespersec|Served bytes per second|rate|Bytes|
|apache_idleworkers|Started workers which do not serve any request|gauge|None|
|apache_reqpersec|Requests per second served by HTTTPD server |rate|OPS|
|apache_totalaccesses|Accesses to server since last restart |counter|None|
|apache_totalkbytes|Bytes served since last restart |counter|kBytes|
