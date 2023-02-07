![HAProxy](../../images/haproxy.png)

##### **Install** 

```commandline
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_haproxy.py ./
```

##### **Configure**

Statistics for HAProxy can be enabled by adding following to haproxy.conf : 

```ini
listen stats
  bind:8888
  mode http
  stats enable
  stats hide-version
  stats realm Haproxy\ Statistics
  stats uri /haproxy?stats
  stats auth User:Pass
```
Then you need to configure Agent to start gathering statistics from Haproxy server. 

```ini
[HAProxy]
url: http://127.0.0.1/haproxy?stats;csv
user: User
pass: Pass
auth: True
upstream: MyApp1
```
Config parameter `upstream` is name of upstream server configured in Haproxy.

```ini
listen  MyApp1 192.168.0.1:80
        option  httpchk
        balance roundrobin
        option httpchk HEAD / HTTP/1.0
        option forwardfor
        cookie  SERVERID insert indirect nocache
        server  app1 192.168.0.10:8888 cookie app1 check inter 10000 fall 3 weight 1
        server  app2 192.168.0.20:8888 cookie app2 check inter 10000 fall 3 weight 1
        capture cookie vgnvisitor= len 32
        rspidel ^Set-cookie:\ IP=       
```
From above mentioned example it is obvious, that upstream name is MyApp1, so this name should be placed as upstream parameter in `HAProxy` section of `webservers.ini`
If you have more sophisticated HAProxy configuration with multiple upstreams and need to monitor several upstreams, you should write comma and separate names in upstream configuration of Agent: 
So if your HAProxy configuration looks like this : 

```yaml
listen  MyApp1 192.168.0.1:80
        option  httpchk
        balance roundrobin
        option httpchk HEAD / HTTP/1.0
        option forwardfor
        cookie  SERVERID insert indirect nocache
        server  app1 192.168.0.10:8888 cookie app1 check inter 10000 fall 3 weight 1
        server  app2 192.168.0.20:8888 cookie app2 check inter 10000 fall 3 weight 1
        capture cookie vgnvisitor= len 32
        rspidel ^Set-cookie:\ IP=       

listen  MyApp2 192.168.0.2:80
        option  httpchk
        balance roundrobin
        option httpchk HEAD / HTTP/1.0
        option forwardfor
        cookie  SERVERID insert indirect nocache
        server  app1 192.168.0.11:8888 cookie app1 check inter 10000 fall 3 weight 1
        server  app2 192.168.0.22:8888 cookie app2 check inter 10000 fall 3 weight 1
        capture cookie vgnvisitor= len 32
        rspidel ^Set-cookie:\ IP=       
```
Agent should be configured as follows: 

```ini
[HAProxy]
url: http://127.0.0.1/haproxy?stats;csv
user: User
pass: Pass
auth: True
```

##### **Restart**

```bash
${OE_AGENT_HOME}/oddeye.sh restart
```

##### **Provides**

| Name  | Description | Type |
| ------------- | ------------- |------------- |
|haproxy_connrate|Connections per second|rate|
|nhaproxy_sessions|Current active sessions |gauge|
