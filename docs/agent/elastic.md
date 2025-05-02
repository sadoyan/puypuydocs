![ElasticSearch](../images/es.png)
 
Agent supports 2 modules for ElasticSearch monitoring:
 
```text
check_elasticsearch.py
check_elasticsearch1x.py
```    
For Clusters of ElasticSearch 1.xx version use check_elasticsearch1x.py (deprecated).
ElasticSearch 2.xx and higher : check_elasticsearch.py module. 

You cannot use both modules on the same machine at the same time.  
In order to enable ElasticSearch, copy or symlink `checks_available` to `checks_enabled` and restart Agent 

ElasticSearch v1 and v2+ have different monitoring API's and thus links for getting statistics are different. 
Both checks will be obvious at `{AGENT_HOME}/config/bigdata.ini` for configuration options.    

#### **ElasticSearch**


##### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_elasticsearch.py ./
```

##### **Configure**

```ini
[ElasticSearch]
host: http://127.0.0.1:9200
stats: /_nodes/_local/stats
```

##### **Restart**

```bash
${PUYPUY_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|elasticsearch_fetch_time|rated time spent on fetching  |rate|Milliseconds|
|elasticsearch_fetch_total|Total time spent on fetching  |gauge|Milliseconds|
|elasticsearch_gc_old_count|Old generation garbage collections count|rate|None|
|elasticsearch_gc_old_time_ms|Old generation garbage collections time |gauge|Milliseconds|
|elasticsearch_gc_young_count|Young generation garbage collections count|rate|None|
|elasticsearch_gc_young_time_ms|Young generation garbage collections time |gauge|Milliseconds|
|elasticsearch_get_time|rated time spent on getting  |rate|Milliseconds|
|elasticsearch_get_total|rated time spent on getting  |gauge|Milliseconds|
|elasticsearch_heap_commited|ElasticSearch JVM heap committed |gauge|Bytes|
|elasticsearch_heap_used|ElasticSearch JVM heap used |gauge|Bytes|
|elasticsearch_http_connections|Current HTTP connections count|gauge|None|
|elasticsearch_index_time|rated time spent on indexing  |rate|Milliseconds|
|elasticsearch_index_total|Total time spent on indexing  |gauge|Milliseconds|
|elasticsearch_merge_docs|Amount of merged docs per second |gauge|None|
|elasticsearch_merge_size|Size of merged docs per second |rate|Bytes|
|elasticsearch_merge_time|Time spent on merging|rate|Milliseconds|
|elasticsearch_non_heap_commited|ElasticSearch non JVM heap committed |gauge|Bytes|
|elasticsearch_non_heap_used|ElasticSearch non JVM heap used |gauge|Bytes|
|elasticsearch_open_files|ElasticSearch daemon open files descriptors count |gauge|None|
|elasticsearch_query_cache_evictions|Query cache evictions count per second|gauge|None|
|elasticsearch_query_cache_hit|Query cache hits count per second|gauge|None|
|elasticsearch_query_query_cache_mis|Query cache miss count per second|gauge|None|
|elasticsearch_refresh_time|rated time spent on refreshing  |rate|Bytes|
|elasticsearch_refresh_total|Total time spent on refreshing  |rate|Milliseconds|
|elasticsearch_search_search_time|rated time spent on searching|rate|Bytes|
|elasticsearch_search_total|Total time spent on searching|rate|Milliseconds|
|elasticsearch_cluster_docs|Total Amount of docs in cluster|gauge|None|
|elasticsearch_cluster_ingest_rate|rate of inserting  documents|rate|OPS|
|elasticsearch_cluster_shards|Total Amount of shards in cluster|gauge|None|
|elasticsearch_cluster_storage_usage|Cluster wide storage usage|gauge|Bytes|

#### **ElasticSearch 1.x**


##### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_elasticsearch1x.py ./
```

##### **Configure**

```ini
[ElasticSearch]
host: http://127.0.0.1:9200
stats: /_nodes/_local/stats/?all=true
```

##### **Restart**

```bash
${PUYPUY_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|elasticsearch_fetch_time|rated time spent on fetching  |rate|Milliseconds|
|elasticsearch_fetch_total|Total time spent on fetching  |gauge|Milliseconds|
|elasticsearch_filter_cache_evictions|Evictions per second from filter cache|rate|None|
|elasticsearch_gc_old_count|Old generation garbage collections count|gauge|None|
|elasticsearch_gc_old_time_ms|Time spent on Old generation garbage collections since last check |gauge|Milliseconds|
|elasticsearch_gc_young_count|Young generation garbage collections count|gauge|None|
|elasticsearch_gc_young_time_ms|Time spent on Young generation garbage collections since last check |gauge|Milliseconds|
|elasticsearch_get_time|rated time spent on getting  |rate|Milliseconds|
|elasticsearch_get_total|Total time spent on getting  |gauge|Milliseconds|
|elasticsearch_heap_committed|ElasticSearch JVM heap committed |gauge|Bytes|
|elasticsearch_heap_used|ElasticSearch JVM heap used |gauge|Bytes|
|elasticsearch_http_connections|Current HTTP connections count|gauge|None|
|elasticsearch_index_time|rated time spent on indexing  |rate|Milliseconds|
|elasticsearch_index_total|Total time spent on indexing  |gauge|Milliseconds|
|elasticsearch_merge_docs|Amount of merged docs per second |rate|None|
|elasticsearch_merge_size|Size of merged docs per second |rate|Bytes|
|elasticsearch_merge_time|Time spent on merging|rate|Milliseconds|
|elasticsearch_non_heap_committed|ElasticSearch non JVM heap committed |gauge|Bytes|
|elasticsearch_non_heap_used|ElasticSearch non JVM heap used |gauge|Bytes|
|elasticsearch_open_files|ElasticSearch daemon open files descriptors count |gauge|None|
|elasticsearch_refresh_time|rated time spent on refreshing  |rate|Milliseconds|
|elasticsearch_refresh_total|Total time spent on refreshing  |gauge|Milliseconds|
|elasticsearch_search_search_time|rated time spent on searching|rate|Milliseconds|
|elasticsearch_search_total|Total time spent on searching|gauge|Milliseconds|
|elasticsearch_cluster_docs|Total Amount of docs in cluster|gauge|None|
|elasticsearch_cluster_ingest_rate|rate of inserting  documents|rate|OPS|
|elasticsearch_cluster_shards|Total Amount of shards in cluster|gauge|None|
|elasticsearch_cluster_storage_usage|Cluster wide storage usage|gauge|Bytes|

