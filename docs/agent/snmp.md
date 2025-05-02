###**Module install**

In order to make **SNMP** negotiation `check_snmp` requires `pysnmp` python module. It can be installed via `pip`. 
Module is tested and properly working with `pysnmp==4.3.9`, older versions may not work properly, 
so please make sure to install`pysnmp==4.3.9` or newer.
To avoid future confusions, we recommend to install exact tested version of **PySNMP**. 

```bash
pip install pysnmp==4.3.9
or 
pip3 install pysnmp==4.3.9
```

`check_snmp` supports v2 **Public** and v3 **authPriv** protocols. Configuration is done in `conf/snmp.ini` file. 

At this moment Agent supports only **SNMP** over **UDP**. Followings are supported authentication and private protocols. 

```text
authProtocol: MD5
authProtocol: SHA
privProtocol: DES
privProtocol: 3DES
privProtocol: AES128
privProtocol: AES192
privProtocol: AES256   
```

`conf/snmp.ini` file already contains commented protocol and auth names, so you need to uncomment ones which matches your **SNMP** servers requirements. 
`check_snmp` module runs similarly to `snmpget` **Net-SNMP** command. So if you run check without authentication, result will be equivalent to : 

```bash
snmpget  -v2c -cpublic 192.168.10.11 .1.3.6.1.4.1.2021.11.9.0
```

For v3 **authPriv** : 
```bash
snmpget -v3 -lauthPriv -u SecurityName -A AuthPassphrase -X PrivPassphrase -a MD5 -x AES 192.168.10.11  1.3.6.1.2.1.25.2.3.1.5.65536
```


Configuration fo **SNMP** plugin is little different that other traditional checks. 
Config parameters are set in `conf/snmp.ini` file, but unlike others naming is little different. 
Names of config sections represents friendly names for you devices. Also you should write names and OIDS of needed checks, 
so Agent can do SNMP get with OID and use friendly names to send information to backend servers.   
```ini
[mikrotik.router.net]
auth: True
port: 161
server: 192.168.10.2
authProtocol: MD5
privProtocol: AES128
SecurityName: demo
AuthPassphrase: Bb123456
PrivPassphrase: Bb123456
total_memory: .1.3.6.1.2.1.25.2.3.1.5.65536
used_memory: .1.3.6.1.2.1.25.2.3.1.6.65536
cpu_frequency: .1.3.6.1.4.1.14988.1.1.3.14.0
tx_rate: .1.3.6.1.4.1.14988.1.1.1.3.1.2.6
rx_rate: .1.3.6.1.4.1.14988.1.1.1.3.1.3.6
client_count: .1.3.6.1.4.1.14988.1.1.1.3.1.6.6
frequency: .1.3.6.1.4.1.14988.1.1.1.3.1.7.6
noise_floor: .1.3.6.1.4.1.14988.1.1.1.3.1.9.6
overall_ccq: .1.3.6.1.4.1.14988.1.1.1.3.1.10.6
```

For better manageability metrics names at endpoint are prefixed with `snmp_` so `if_eth0_out_errors` 
will be shown as `snmp_if_eth0_out_errors` in Dashboard.    

**Please make sure to create correct mappings before running  `check_snmp`**

###**SNMP v2 Public** 

`auth: Boolean` parameter in `conf/snmp.ini` controls switching protocol an community versions. 

```ini
auth: False
```

Will set to legacy v2 protocol with **Public** community. As result you will have **SNMP** querly similar to :   

```bash
snmpget  -v2c -cpublic 192.168.10.11 .1.3.6.1.4.1.2021.11.9.0
```
 
`auth: True` enables v3 AuthPriv .  
 
###**SNMP v3 AuthPriv** 

V3 is enhanced Protocol of SNMP, which supports encryption and authentication.
Our agent supports following protocols.

```text
authProtocols, MD5, SHA
privProtocols, DES, 3DES, AES128, AES192, AES256
```
 
Please setup `authProtocol` and `privProtocol` values to ones which matches your SNMP servers requirements.
Eventual config file will look something like this : 
```ini
[linux.router.net]
auth: True
port: 161
server: 192.168.10.11
authProtocol: SHA
privProtocol: DES
SecurityName: demo
AuthPassphrase: Bb1234567
PrivPassphrase: Bb1234567
mem_available: .1.3.6.1.4.1.2021.4.6.0
```

As result you will have **SNMP** querly similar to : 

```bash
snmpget -v3 -lauthPriv -u user -A AuthpaSS -X ProtopaSS -a SHA -x DES 192.168.10.11  .1.3.6.1.4.1.2021.4.6.0
```

After configuration and mapping is finished, you can enable `check_snmp` as any other **PuyPuy** check and restart Agent: 

```bash
cd checks_enabled
ln -s ../checks_available/check_snmp.py ./
../puypuy.sh restart 
```
