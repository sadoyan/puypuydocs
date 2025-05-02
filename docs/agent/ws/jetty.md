![Jetty](../../images/jetty.png)

##### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_jetty.py ./
```

##### **Configure**


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

##### **Provides**

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

