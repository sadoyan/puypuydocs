![Solr](../images/solr.png)

**Solr** check is tested with Vanilla (**Apache** version) of **Solr** based on **Jetty**.
Out of the box installtion of **PuyPuy Agent** may not work properly with other ditrinution of **Solr** (Cloudera, Tomcat, etc ...)   

##### **Install**

```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_solr.py ./
```

##### **Configure**

Configuration of **Solr** check is located at `bigdata.ini`. By default it will look fo **Solr** installation `localhost`. 
Please make sure to set correct IP address of **Solr** instance which you want to monitor.   

```ini
[Solr]
stats: http://127.0.0.1:8983/solr/admin/metrics?group=all&wt=json
```

##### **Restart**

```bash
${OE_AGENT_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|solr_1xx_responses|HTTP 1xx responses per second|rate|OPS|
|solr_2xx_responses|HTTP 2xx responses per second |rate|OPS|
|solr_3xx_responses|HTTP 3xx responses per second |rate|OPS|
|solr_4xx_responses|HTTP 4xx responses per second |rate|OPS|
|solr_5xx_responses|HTTP 5xx responses per second |rate|OPS|
|solr_delete_requests|HTTP DELETE requests |rate|OPS|
|solr_gc_{cms/g1_old}_count|CMS or G1 Old gen Garbage Collections count |counter|None|
|solr_gc_{cms/g1_old}_time|CMS or G! Old gen Garbage Collections time|rate|Milliseconds|
|solr_gc_{parnew/g1_young}_count|CMS or G1 Young gen Garbage Collections count |counter|None|
|solr_gc_{parnew/g1_young}_time|CMS or G1 Young gen Garbage Collections count |rate|Milliseconds|
|solr_heap_committed|Java Heap committed |counter|Bytes|
|solr_heap_init|Java Heap init|counter|Bytes|
|solr_heap_max|Java Heap max|counter|Bytes|
|solr_heap_used|Java Heap used|counter|Bytes|
|solr_non_heap_committed|Java Non Heap committed |counter|Bytes|
|solr_non_heap_init|Java Non Heap init|counter|Bytes|
|solr_non_heap_max|Java Non Heap max|counter|Bytes|
|solr_non_heap_used|Java Non Heap used|counter|Bytes|
|solr_move_requests|MOVE requests executed on node |rate|OPS|
|solr_options_requests|OPTIONS requests executed on node |rate|OPS|
|solr_get_requests|GET requests executed on node |rate|OPS|
|solr_put_requests|PUT requests executed on node |rate|OPS|
|solr_head_requests|HEAD requests executed on node |rate|OPS|
|solr_other_requests|Other requests executed on node |rate|OPS|
|solr_trace_requests|Trace requests executed on node |rate|OPS|
|solr_requests_all|Requests executed on node |rate|OPS|
|solr_threads|Currently running Java threads |gauge|None|
|solr_threads_daemon|Currently running Java daemon threads |gauge|None|


