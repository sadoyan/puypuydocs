**Custom Checks**
---------
Create file in checks_enabled directory with name check_checkname.py, 
inside script you should have class **Check** which is inherited from **lib.basecheck.CheckBase** and function precheck(self) which will override precheck() in main class.  (Here you actual check should live). 
Your check should contain some  minimal imports in order to talk to main program:
Example below is simple check which gets system's Load Average and sends it to PuyPuy  

```python
import lib.getconfig
import lib.pushdata
import lib.basecheck
import lib.puylogger

check_type = 'system'
reaction = -3

class Check(lib.basecheck.CheckBase):

    def precheck(self):
        try:
            loadavg = open("/proc/loadavg", "r")
            proc_loadavg = loadavg.readline().split()
            self.local_vars.append({'name': 'sys_load_1', 'timestamp': self.timestamp, 'value': proc_loadavg[0]})
            self.local_vars.append({'name': 'sys_load_5', 'timestamp': self.timestamp, 'value': proc_loadavg[1]})
            self.local_vars.append({'name': 'sys_load_15', 'timestamp': self.timestamp, 'value': proc_loadavg[2]})
            loadavg.close()
        except Exception as e:
            lib.pushdata.print_error(__name__ , (e))
            pass
```

This will import libs to generate data for back-end needed, so you do not have to deal with generating and pushing it manually. 


**Create scripts which calculates value rates :** 

```python
import lib.getconfig
import lib.pushdata
import lib.basecheck
import lib.puylogger

check_type = 'system'
reaction = -3

class Check(lib.basecheck.CheckBase):

    def precheck(self):
	    local_vars = []
	    rate=lib.record_rate.ValueRate()
	    timestamp = int(datetime.datetime.now().strftime("%s"))
		name1 = 'SomeName'
		name2 = 'OtherName'
	    value1 = 10
	    value2 = 20
	    value_rate1 = self.rate.record_value_rate(name1, value1, self.timestamp)
	    value_rate2 = self.rate.record_value_rate(name2, value2, self.timestamp)
	    self.local_vars.append({'name': name1, 'timestamp': self.timestamp, 'value': value_rate1})
        self.local_vars.append({'name': name2, 'timestamp': self.timestamp, 'value': value_rate2})
```


### Custom Script 

To run custom script like Bash, Perl etc.. Create scripts in format *check_name.extension* in folder scripts_enabled. 
All is needed from custom is to system out values in right order, Below is sample Bash script, 
which generates random number and send to collector for graphing: Make sure to have check_style parameter (stack/rate). 
This is for telling main program if it should calculate value rates or just push data as it comes. 

```bash
	#!/bin/bash
		
	myvalue=$RANDOM
	mytype=random_gen
	check_type=my_bash_random
	check_style=stack
	
	myvalue2=$RANDOM
	mytype2=random_gen2
	check_type2=my_bash_random2
	check_style2=rate
	
	echo $mytype $myvalue $check_type $check_style
	echo -n $mytype2 $myvalue2 $check_type2 $check_style2
```

### PuyPuy Specific 

As PuyPuy is completely push based and our servers does not have any direct access to your infrastructure, 
we need special check which will determine if particular host is alive or not. Thus we made small module, 
which will call our servers and get and send response times to PuyPuy. 
We will generate host alive parameter based based on information sent by `check_puypuy.py`. 
If you want to have host aliveness test, enable check_puypuy.py as you will do with any other python check and restart Agent:

```bash
cd ${agent_home}/checks enabled
ln -s ../checks-available/check_puypuy.py ./ 
```

PuyPuy is dynamic system based on machine learning, 
but if you want to have statically defined alerts you can use `send_special` method in your python module. 
Example below demonstrates how custom alerts can be configured, its taken from **check_network_bytes** module: 

```python
import glob
import lib.getconfig
import lib.pushdata
import lib.record_rate
import lib.puylogger
import lib.basecheck

check_type = 'system'
check_localhost = lib.getconfig.getparam('Network Stats', 'localhost')
rated = lib.getconfig.getparam('Network Stats', 'rated')


class Check(lib.basecheck.CheckBase):
    
    def precheck(self):
        try:
            ifaces = glob.glob("/sys/class/net/*")
            iflist = []
            for index in range(0, len(ifaces)):
                if check_localhost is False:
                    iface = ifaces[index].split('/')[4]
                    if "/lo" not in ifaces[index]:
                        iflist.append(iface)
                else:
                    iface = ifaces[index].split('/')[4]
                    iflist.append(iface)

            for nic in iflist:
                rxb = open("/sys/class/net/" + nic + "/statistics/rx_bytes", "r")
                txb = open("/sys/class/net/" + nic + "/statistics/tx_bytes", "r")
                rx = int(rxb.read())
                tx = int(txb.read())

                if rx is not 0 or tx is not 0:
                    txname = 'bytes_tx'
                    rxname = 'bytes_rx'
                    if rated is True:
                        rxrate = self.rate.record_value_rate(rxname + nic, rx, self.timestamp)
                        txrate = self.rate.record_value_rate(txname + nic, tx, self.timestamp)
                        self.local_vars.append({'name':rxname, 'timestamp': self.timestamp, 'value':rxrate, 'chart_type': 'Rate', 'check_type': check_type, 'reaction': 0, 'extra_tag':{'device': nic}})
                        self.local_vars.append({'name':txname, 'timestamp': self.timestamp, 'value':txrate, 'chart_type': 'Rate', 'check_type': check_type, 'reaction': 0, 'extra_tag':{'device': nic}})
                    else:
                        self.local_vars.append({'name':rxname, 'timestamp': self.timestamp, 'value':rxrate, 'chart_type': 'Counter', 'check_type': check_type, 'reaction': 0, 'extra_tag':{'device': nic}})
                        self.local_vars.append({'name':txname, 'timestamp': self.timestamp, 'value':txrate, 'chart_type': 'Counter', 'check_type': check_type, 'reaction': 0, 'extra_tag':{'device': nic}})

                rxb.close()
                txb.close()
        except Exception as e:
            lib.pushdata.print_error(__name__ , (e))
            pass

```
        
You can disable or change dynamic alerting for particular checks, by passing `reaction` parameter to `jsondata.gen_data` . 
Arguments for reaction parameter are followings: 
```text
reaction = -3 # (Disables dynamic alerting and learning on this check)
reaction = -1 # (Disables dynamic alerting if values are smaller than expected)
reaction = -2 # (Disables dynamic alerting if values are bigger than expected)
reaction = 0  # (Default : Enable dynamic alerting and learning on this check)
jsondata.gen_data(txname, timestamp, value, lib.pushdata.hostname, check_type, cluster_name, reaction)
```

We have created another **PuyPuy** specific optional parameter. 
This is to tell back-end type of incoming messages. 
It accepts "Rate" and "Counter" arguments.

**Rate**: is used to tell **PuyPuy** that incoming metrics are rated so back-end knows **better** how to calculate  dynamic rules.

**Counter**: Is set when we have increasing counter for metrics values. 

This is needed to do better calculation of regression, and to reset it when next value is smaller than previous. 
If you find this not suitable for your needs, do not set parameter even when your metrics values are increasing counter, 
but keep in mind to manually drop regression when you counter resets. 
Otherwise regression counter will think that something happens as new values are out of expected regression scopes 
and will set high level of alerting to check. 
