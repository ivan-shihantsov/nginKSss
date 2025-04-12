## Multiple static web-sites on one server

Assume, your server IP is 192.168.1.15

There are 3 way to serve multiple web-sites on your server:
1) have multiple net interfaces with their own IP or multiple IP addresses on one interface:
    * site1: 192.168.1.15
    * site2: 192.168.1.16
2) set each site to its own port (with the same one IP):
    * site1: 192.168.1.15:8080
    * site2: 192.168.1.15:8081
3) assign each server as a sub-path for one IP:
    * 192.168.1.15/site1
    * 192.168.1.15/site2
4) mix
    * site0: 192.168.1.15
    * site1: 192.168.1.16
    * site2: 192.168.1.17:8080
    * site3: 192.168.1.17:8081
    * site4: 192.168.1.18/site4
    * site5: 192.168.1.18/site5
    * site6: 192.168.1.19:8080/site6
    * site7: 192.168.1.19:8080/site7
    * site8: 192.168.1.19:8081/site8
    * site9: 192.168.1.19:8081/site9

----

## Case 1

This is a solution for public servers. You need to buy additional IP and possibly it will look like a separate interface

For your project in the LAN you can add one more web-site on the same pc (server) you need to add the 2nd IP on the network interface
```
sudo ip addr add 192.168.1.16/24 dev <eth0>
```

use this [general nginx config](case1/nginx.conf) and configs for [site1](case1/sites-available/site-burger.conf) and [site2](case1/sites-available/mysite2)


1. it doesn't matter what are the names of your sites<br>
    * I have `burger` (site 1) and `story` (site 2) - downloaded as .zip (include index.html)

2. it doesn't matter where you move your sites to<br>
    * /var/www/a-burger/
    * /srv/a-story/

3. it doesn't matter how you name site configs at /etc/nginx/sites-available/ and link to them at /etc/nginx/sites-enabled/<br>
    * **copy configs from this dir**
    * * /etc/nginx/sites-available/site-burger.conf
    * * /etc/nginx/sites-available/mysite2
    * **its name can include `.conf`, but may not**
    * * just main config must `include /etc/nginx/sites-enabled/*;`
    * **create links**
    * * ln -s /etc/nginx/sites-available/site-burger.conf /etc/nginx/sites-enabled/burga
    * * ln -s /etc/nginx/sites-available/mysite2 /etc/nginx/sites-enabled/sorry.conf

```
# ls -l /etc/nginx/sites-available/
mysite2
site-burger.conf
```

```
# ls -l /etc/nginx/sites-enabled/
burga -> /etc/nginx/sites-available/site-burger.conf
sorry.conf -> /etc/nginx/sites-available/mysite2
```


default options which I even needn't to change
* search `index.html` by default
* default port is `80`

----

## Case 2

One IP - Multiple ports<br>
Setup the same as for case 1 ([general nginx config](case2/nginx.conf) is also similar) - just add little edits to [site1](case2/sites-available/site-burger.conf) and [site2](case2/sites-available/mysite2)<br>

Now you can open sites:
* site1: 192.168.1.15:8080
* site2: 192.168.1.15:8081

If not, set firewall rules to allow traffic. Or ports can be busy
```
sudo ufw allow 8080
sudo ufw allow 8081
```

this approach isn't an enterprise solution for clients (end users), but it's very useful on production - for internal tools/components, for developers, as part of docker compose or Kubernetes clusters<br>

----

## Case 3

This is the case for pages of on complex web-site or for web-apps to make possible requests from clients to server API<br>

Setup is the same as for case 1 - only replace the ([general nginx config](case3/nginx.conf).<br>

In this case we can make different request path and real files location<br>
But server must be one for both sites = one common config in sites-available. I did not create it - let's do it in general nginx config<br>

Now you can open your web-sites:
* http://192.168.100.101/burbur/
* http://192.168.100.101/stories/

----



