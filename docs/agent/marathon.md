
![Marathon](../images/mesos-marathon.png)  

### **Marathon**

Marathon master exposes metrics via HTTP/Json out of the box, so all you need is just to symlink `checks_available/check_marathon.py` to `checks_enabled` and restart Agent.

##### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_marathon.py ./
```

##### **Configure**

Configuration of Marathon check is Marathon section of `conf/bigdata.ini` 
You need to set full URL of statistics interface of marathon which is : http://{HOST}:{PORT}/metrics 

```ini
[Marathon]
stats: http://127.0.0.1:8888/metrics
```

##### **Restart**
```bash
${PUYPUY_HOME}/puypuy.sh restart
```

##### **Provides**

| Name  | Description | Type | Unit|
| ------------- | ------------- |------------- |------------- |
|marathon_apps_active|Amount of currently active applications|gauge|None|
|marathon_deployments_active|Amount of currently active deployments|gauge|None|
|marathon_gc_collections|Garbage collations performed bt Marathon daemon|counter|None|
|marathon_gc_duration|Time spend on GC my Marathon daemon since it's start|counter|None|
|marathon_groups_active|Amount of currently active groups|gauge|None|
|marathon_heap_committed|Marathon JVM heap committed|gauge|Bytes|
|marathon_heap_used|Marathon JVM heap used|gauge|Bytes|
|marathon_http_event_streams_active|Amount of currently active HTTP event streams|gauge|None|
|marathon_http_requests_active|Amount of currently active requests|gauge|None|
|marathon_instances_launch_overdue|Overdue of launch instances|gauge|None|
|marathon_instances_running|Currently running instances|gauge|None|
|marathon_instances_staged|Currently staged instances|gauge|None|
|marathon_nonheap_committed|Marathon non JVM heap committed|gauge|Bytes|
|marathon_nonheap_used|Marathon non JVM heap used|gauge|Bytes|
|marathon_pods_active|Currently active pods|gauge|None|

