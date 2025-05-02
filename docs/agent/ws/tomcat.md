![Tomcat](../../images/tomcat.png)

##### **Install**

```bash
cd ${PUYPUY_HOME}/checks_enabled
ln -s ../checks_available/check_tomcat.py ./
```

##### **Configure**

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

##### **Restart**

```bash
${PUYPUY_HOME}/puypuy.sh restart
```

##### **Provides**

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