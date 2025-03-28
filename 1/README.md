## static web-site installation

we are inside vbox ubuntu
here site is installed
1) /etc/nginx/nginx.conf
2) site directories hold in ~/workdir/www/

```
# ls -las /etc/nginx/sites-available/
4 -rw-r--r-- root root default
4 -rw-r--r-- root root mysite2
4 -rw-r--r-- root root site-burger.conf
```

```
# ls -las /etc/nginx/sites-enabled/
0 lrwxrwxrwx root root burger -> /etc/nginx/sites-available/site-burger.conf
0 lrwxrwxrwx root root default -> /etc/nginx/sites-available/default
0 lrwxrwxrwx root root wind.conf -> /etc/nginx/sites-available/mysite2
```

host system has only 1 record
```
/etc/hosts
192.168.23.47  wind.local
192.168.23.47  burger.local
```