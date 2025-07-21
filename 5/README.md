## Simpliest Proxy config

assume, your server with web-site has IP 192.168.1.10<br>
and your proxy-servers have IPs 192.168.1.21 and 192.168.1.22<br>

nginx-1.conf - config for `main` server with web-site<br>
now you can show it in browser: `http://192.168.1.10` <br>

nginx-2.conf - config for `server2` which is proxy<br>
now you can show web-site via this proxy in browser: `http://192.168.1.21` <br>

nginx-3.conf - config for `server3` which is proxy for another proxy<br>
now you can show web-site via this proxy in browser: `http://192.168.1.22` <br>


and now you have the current chain of reqests:<br>
[your PC] -> [`server3` (192.168.1.22)] -> [`server2` (192.168.1.21)] -> [`main` server (192.168.1.10)] <br>

if you turn off `server2` and make request to `server3` again, you receive `ERR_ADDRESS_UNREACHABLE` "This site can’t be reached"

![do not forget to update pic when update the scheme file](res/scheme.png "scheme") <br>

