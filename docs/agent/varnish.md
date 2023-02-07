##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_varnish.py ./
```

##### **Configure**

Configuration of **Varnish** check is located at `webservers.ini`. 
You only need to set right path for ```varnishstat``` binary, by default for packaged installation it is ```/usr/bin/varnishstat``` .
If you have compilled **Varnish** from source or used oher mechanism to install it, just locate ```varnishstat``` change default path in config file. 

Restart Agent. `./oddeye.sh restart`
`check_varnish` module should run without making changes in configuration, but according to your specific needs, you can edit `conf/bindata.ini` and make changes in section **Varnish**

```ini
[Varnish]
varnishstat: /usr/bin/varnishstat
varnishuser: varnish
```

Usually ```varnishstat``` requires ether root or {VARNISH_USER} privilege, 
so user which runs **OddEye Agent** should be able to execute ```varnishstat```  on behalf of privileged user. 
In most of linux distributions adding following line to ```/etc/sudoers``` will do the trick. Lets assume that **Varnish** is running under user ```varnish```

```bash
oddeye    ALL = (varnish) NOPASSWD: /usr/bin/varnishstat
```

Now restart **OddEye Agent** 

```bash
${OE_AGENT_HOME}/oddeye.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|sess_conn|Cumulative number of accepted client connections by Varnish Cache|rate| OPS|
|client_req|Cumulative number of received client requests. Increments after a request is received, but before Varnish responds|rate| OPS|
|cache_hit|Cumulative number of times a file was served from Varnish’s cache|rate| OPS|
|cache_miss|Cumulative number of times a file was requested but was not in the cache, and was therefore requested from the backend|rate| OPS|
|cache_hitpass|Cumulative number of hits for a “pass” file|rate| OPS|
|threads|Number of threads in all pools|current |None|
|threads_created|Number of times a thread has been created|current |None|
|threads_failed|Number of times that Varnish unsuccessfully tried to create a thread|current |None|
|threads_limited|Number of times a thread needed to be created but couldn't because varnishd maxed out its configured capacity for new threads|current |None|
|thread_queue_len|Current queue length: number of requests waiting on worker thread to become available|current |None|
|sess_queued|Number of times Varnish has been out of threads and had to queue up a request|current |None|
|backend_conn|Cumulative number of successful TCP connections to the backend|counter |None|
|backend_recycle|Cumulative number of current backend connections which were put back to a pool of keep-alive connections and have not yet been used|counter |None|
|backend_reuse|Cumulative number of connections that were reused from the keep-alive pool|counter |None|
|backend_toolate|Cumulative number of backend connections that have been closed because they were idle for too long|counter |None|
|backend_fail|Cumulative number of failed connections to the backend|counter |None|
|backend_unhealthy|Cumulative number of backend connections which were not attempted because the backend has been marked as unhealthy|counter |None|
|backend_busy|Cumulative number of times the maximum amount of connections to the backend has been reached|counter |None|
|backend_req|Number of requests to the backend|counter |None|
|n_expired|Cumulative number of expired objects for example due to TTL|gauge|None|
|n_lru_nuked|Least Recently Used Nuked Objects: Cumulative number of cached objects that Varnish has evicted from the cache because of a lack of space|current |None|
|sess_dropped|Number of connections dropped due to a full queue|current |None|
