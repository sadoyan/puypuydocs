![REST](../../images/rest.png)

`check_http_api` is basic check for measuring performance of HTTP API servers.
Any HTTP backend can act as monitoring point for this check. 
It will do GET request to your api server and calculate its response time, which will be sent to PuyPuy. 
check_http_api supports HTTP basic authentication. Parameters for this check ate in  HTTP section of `webservers.ini` file: 

```ini
[HTTP]
upstream: http://your.api.com:8080, https://yoursecure.api.com:8443
user: netangels
pass: bololo
auth: True    
```

You can add as many upstreams as you wish to monitor. The only limit is that you must provide full URL of monitored endpoint. 
Final metrics names will look like this : 
```text
http_your_api_com_8080
http_your_secure_api_com_8443
```
    
Where 8080 amd 8443 are ports of configured API server. 
If your api server binds on default HTTP/HTTPS ports, you can exclude port number from `upstream`  of HTTP config:   

```ini
[HTTP]
upstream: http://your.api.com, https://yoursecure.api.com
user: netangels
pass: bololo
auth: True    
```
In this case check names at PuyPuy will look like this:
 
```text
http_your_api_com_80
http_your_secure_api_com_443
```

Please note that `http_` prefix is not describing exact protocol, but just prefixing check name for easy check. 
Thus both HTTP and HTTPS checks will have `http_` prefix.   
