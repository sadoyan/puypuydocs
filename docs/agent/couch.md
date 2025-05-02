**CouchBase**
---------


**Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_couchbase_{VERSION}.py ./
```

**Configure**

Edit `${PUYPUY_HOME}/conf/bigdata.ini` and change auth parameters and ip address of Couchbase servers to values matching your actual node running node parameters. 
Also its required to write comma separated list of buckets which you want to monitor, so please make sure to change `buckets:` to names of buckets which  you want to monitor.  

```ini
[CouchBase]
stats: http://127.0.0.1:8091/pools/default/buckets
buckets : default

[CouchBase5x]
stats: http://127.0.0.1:8091/pools/default/buckets
buckets : beer-sample, gamesim-sample, travel-sample
user: admin
pass: adminadmin
auth: True
```

**Restart**

```bash
${PUYPUY_HOME}/puypuy.sh restart
```

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|couchbase_clusterwide_itemcount|Per bucket clusterwide items count |gauge|None|
|couchbase_clusterwide_memused|Per bucket used memory fo entire cluster|gauge|None|
|couchbase_clusterwide_opspersec|OPeration per second executed on entire cluster|rate|OPS|
|couchbase_clusterwide_quotapercentused|Per bucker quota usage for entire cluster |gauge|Percent|
|couchbase_cmd_get|Per buicket GETs executen on current node|rate|OPS|
|couchbase_couch_docs_data_size|Size of couch documents associated with a node|gauge|Bytes|
|couchbase_curr_items|Amount of items on current node|gauge|None|
|couchbase_curr_items_tot|Write bytes per second for entire cluster|gauge|None|
|couchbase_ep_bg_fetched|Number of disk fetches performed on node|counter|None|
|couchbase_get_hits|Get hits performent on current node |rate|OPS|
|couchbase_mem_used|Current node's used memory|gauge|Bytes|
|couchbase_ops|Operations per second performed against entire cluster |gauge|OPS|
|couchbase_vb_replica_curr_items|Number of replicated items/documents|curent |None|

**CouchDB**
---------


**Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_couchdb_{VERSION}.py ./
```

**Configure**

Edit `${PUYPUY_HOME}/conf/bigdata.ini` and change auth parameters and ip address of CouchDB servers to values matching your actual node running node parameters. 
For CouchDB 2x check you can set  parameter `detailed` to `True/False`. 
When detailed is set to True agent will send metrics about HTTP status and response codes and, which will significantly increase number of metrics.     

```ini
[CouchDB]
stats: http://127.0.0.1:5984/_stats

[CouchDB2]
stats: http://127.0.0.1:5984
user: admin
pass: admin
auth: True
detailed = True
```

**Restart**

```bash
${PUYPUY_HOME}/puypuy.sh restart
```

**Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|couchdb_bulk_requests|Bulk requests per second on current node|rate|OPS|
|couchdb_database_reads|Database reads per second on current node |rate|OPS|
|couchdb_database_writes|Database writes per second on current node |rate|OPS|
|couchdb_document_inserts|Inserted documents per second on current node|rate|OPS|
|couchdb_document_writes|Written documents per second on current node|rate|OPS|
|couchdb_requests|Total maount of requests per second on current node|rate|OPS|
|couchdb_requests_methods|Amount of http requests on curent node by HTTP methods |rate|OPS|
|couchdb_status_codes|Amount of http requests on curent node by HTTP status codes|rate|OPS|
|couchdb_temporary_view_reads|Temperory view reads on current node per second |rate|OPS|
|couchdb_view_reads|View reads on current node per second |rate|OPS|

 



