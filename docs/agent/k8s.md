![kubernetes](../images/kubernetes.png)

***OddEye*** Kubernetes plugin requires up and running  **Kube State Metrics server**    

##### **Install**

Installation of Kube State Metrics server is easy and can be done by executing a single command :  


```bash
go get k8s.io/kube-state-metrics
```

This will Install Kube State Metrics server to your $GOPATH using go get:
You can find more information about More info about Kube State Metrics server here : [***https://github.com/kubernetes/kube-state-metrics***](https://github.com/kubernetes/kube-state-metrics)

You can run Kube State Metrics in foreground by executing : 

```bash
$GOPATH/bin/kube-state-metrics --port=6868 --telemetry-port=8081 --kubeconfig=/etc/kubernetes/admin.conf
```

It's recommended to create systemd service and run Kube State Metrics on startup; 

```bash
cat > /etc/systemd/system/kube-state-metrics.service <<-EOF
[Unit]
Description=Kube State Metrics
Documentation=https://github.com/kubernetes/kube-state-metrics
Wants=network-online.target
After=network-online.target

[Service]
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/usr/local/bin/kube-state-metrics --port=6868 --telemetry-port=8081 --kubeconfig=/etc/kubernetes/admin.conf
KillMode=process
KillSignal=SIGINT
Restart=always
RestartSec=2
StartLimitBurst=3
StartLimitIntervalSec=10
TasksMax=infinity

[Install]
WantedBy=multi-user.target
EOF
```

Reload systemd and enable service . 

```bash
systemctl  daemon-reload  
systemctl  enable kube-state-metrics.service 
systemctl  start  kube-state-metrics.service 
```

Now **Kube State Metrics server** is controlled by systemd and will start in background during OS boot. 

Enable **OddEye** agent check :
 
```bash
cd ${OE_AGENT_HOME}/checks_enabled
ln -s ../checks_available/check_kube_state_metrics.py ./
```

##### **Configure**

If you are using default installation, no additional configuration is needed.
If you need to monitor remote server or you **Kube State Metrics**  server is running on different IP:PORT, 
edit `conf/k8s.ini` section `kube-state-metrics` and set `metrics` parameter with value matching your needs.
You can enable/disable per pod statistics collection by setting ```podinfo : False/True``` (Default is True)  

```ini
[kube-state-metrics]
metrics : http://127.0.0.1:6868/metrics
podinfo : True
```

##### **Restart**

```bash
${OE_AGENT_HOME}/oddeye.sh restart
```

##### **Provides**

| Name  | Description | Type |
| ------------- | ------------- |------------- |
|kube_deployment_status_replicas|The number of replicas per deployment.|gauge|
|kube_deployment_status_replicas_available|The number of available replicas per deployment.|gauge|
|kube_deployment_status_replicas_unavailable|The number of unavailable replicas per deployment.|gauge|
|kube_deployment_status_replicas_updated|The number of updated replicas per deployment.|gauge|
|kube_endpoint_address_available|Number of addresses available in endpoint.|gauge|
|kube_pod_container_resource_limits_cpu_cores|The limit on cpu cores to be used by a container.|gauge|
|kube_pod_container_resource_limits_memory_bytes|The limit on memory to be used by a container in bytes.|gauge|
|kube_pod_container_status_waiting|Describes whether the container is currently in waiting state.|gauge|
