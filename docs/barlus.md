**OddEye Api**
--------------

REST API severs of OddEye. It talks Json so you can use any http client to push metrics to OddEye.
To do this you need to have valid OddEye account with UUID. Below is sample curl command and Json file: 

###Curl example

    curl -i -XPOST 'https://api.oddeye.co/oddeye-barlus/put/tsdb' --data-binary 'UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&data={JsonData}'

As **OddEye** stands for Open API you can generate your own checks, all you need is to pack in in Json and use HTTP POST method to send it to **Api**

###Json Sample
```json
      [
        {
            "metric": "rated_metric_name",
            "reaction": 0,
            "tags": {
                "cluster": "testcluster",
                "group": "testing",
                "host": "host.domain.com",
                "type": "mytype"
            },
            "timestamp": 1492526216,
            "type": "Rate",
            "value": 12
        },
        {
            "metric": "notype_metric_name",
            "reaction": 0,
            "tags": {
                "cluster": "testcluster",
                "group": "testing",
                "host": "host.domain.com",
                "type": "mytype"
            },
            "timestamp": 1492526216,
            "type": "None",
            "value": 55
        }
    ]
``` 
Metrics can be sent in separate Jsons as well as combined to one bigger Json.  
If everything is ok, you should get code 200 from server. 

### Error Codes

For handling OddEye specific cases we use several standard, but not widely usable error codes:

```text
424 \"FAILURE\",messge:\"NOT VALID UUID\"
406 \"FAILURE\",messge:\"EMPTY UUID\"
411 \"FAILURE\",\"message\":\"Not valid data\"
415 \"FAILURE\",\"message\":\" Unknown error \"
424 \"FAILURE\",\"message\":\"Empty Data\"`
402 \"FAILURE\",\"message\":\"Payment Required\"`
```

### Special Parameters

In order to have more robust control of anomaly detection and alerting, **Api** accepts control tags from client,
If these parameters does exists, OddEye will set to defaults values. 
These are `reaction` and `type` params. 

`reaction` is for setting up machine learning and alerting parameters: 

```bash
reaction = -3 # (Disables dynamic alerting and learning on this check)
reaction = -1 # (Disables dynamic alerting if values are smaller than expected)
reaction = -2 # (Disables dynamic alerting if values are bigger than expected)
reaction =  0 # (Default : Enable dynamic alerting and learning on this check)
```

`types` will help **OddEye** "understand" incoming metric types. 
For example if `type` is set to `Counter` **OddEye** will tune counting of regression and reset it when new metric have lower value than previous one.
This is usable when you are sending increasing counters like access to your application, which will be zeroed when application restarts.
In this situation if parameter `Counter` is not set, **OddEye** will not know that zeroed value is expected, thus will set higher anomaly rate on it.

```bash
type = Rate (For sending rated metrics)
type = Percent (For sending metrics which are 1-100 percents of something)
type = Counter (For sending metrics as increasing counters)
type = None (Default)
```

In addition to this **OddEye** also accepts "Special" metrics for error handling and alert generation. 
Following is "Special" json which tells **OddEye** that system Load-Average has **Critial** value and action from DevOps is required:

```json
    [
        {
            "message": "ERROR: System Load average is at 123.25 percent of available  resources",
            "metric": "Load-Average",
            "reaction": 0,
            "status": "ERROR",
            "tags": {
                "cluster": "testcluster",
                "group": "testing",
                "host": "hobbit"
            },
            "timestamp": 1497549387,
            "type": "Special",
            "value": 0
        }
    ]
```

If `status` is **ERROR** you will see message with **Severe** severity at top of monitoring page. 
When status is changed **OddEye** will remove **Severe** message from Dashboard after 2 minutes. 

### Error Handling

**Error** messages ara much like **Special** ones, but main goal of it is to detect if monitorable service is responding normally or no. 
Agent will sends it automatically, but if you write you own agent, make sure that you generate **Error** message in exception block.
This is not mandatory, but it will alert **OddEye** that monitorable application is not responding, so **OddEye** can generate appropriate message in Dashboard.  

Sample error message looks like this:

```json
    [{
        'status': 'ERROR',
        'reaction': -2,
        'value': 16,
        'timestamp': 1497550147,
        'tags': {
            'cluster': 'testcluster',
            'group': 'testing',
            'host': 'hobbit'
        },
        'metric': 'check_nginx',
        'type': 'Special',
        'message': '7, Failed to connect to 127.0.0.1 port 80: Connection refused'
    }]
```

This message will tell **OddEye** that NginX server is not running so **OddEye** Dashboard will show **Severe** alert at the top of monitoring page. 
