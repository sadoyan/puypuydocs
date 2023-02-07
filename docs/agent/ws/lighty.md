![Lighttpd](../../images/lighty.png)

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_lighttpd.py ./
```

##### **Configure**

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
