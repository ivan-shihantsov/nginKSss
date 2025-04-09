## minimal site config

assume, your server IP is 192.168.1.15

```
move your site to the /var/www/ dir
# cp -r ~/mysite /var/www/

set /etc/nginx/nginx.conf

sudo systemctl reload nginx
sudo systemctl restart nginx

open http://192.168.1.15 in browser
```

<br>

----

even no need for some directives since they are covered by default:
<br>
server {<br>

* `listen 80;`
* `server_name mydomain.com;`
* `index index.html;`
* `location / { try_files $uri $uri/ =404; }`<br>
}

