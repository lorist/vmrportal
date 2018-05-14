# vmrportal
Documentation:
http://loristweb.s3-website-ap-southeast-2.amazonaws.com/vmrportal/index.html

For later version of Ubuntu (16+) and running behind Nginx, I have managed to get this working by following:

https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04


## /etc/systemd/system/vmrportal.service:

```
[Unit]
Description=uWSGI instance to serve vmrportal
After=network.target

[Service]
User=pexip
Group=www-data
WorkingDirectory=/home/pexip/vmrportal
Environment="PATH=/home/pexip/vmrportalvenv/bin"
ExecStart=/home/pexip/vmrportalvenv/bin/uwsgi --ini portal.ini

[Install]
WantedBy=multi-user.target
```

## /etc/nginx/sites-enabled/vmrportal:

```
# HTTPS server
#
server {
        listen 443;
        server_name portal.customer.com 10.10.10.10;

        root html;
        index index.html index.htm;

        ssl on;
        ssl_certificate ssl/cert.pem;
        ssl_certificate_key ssl/cert.key;

        ssl_session_timeout 5m;

        ssl_protocols SSLv3 TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers "HIGH:!aNULL:!MD5 or HIGH:!aNULL:!MD5:!3DES";
        ssl_prefer_server_ciphers on;

        location / {
            return 301 /portal;
            }
        location /static {
            alias /home/pexip/vmrportal/static;
            }
        location /portal { try_files $uri @yourapplication; }

        location @yourapplication {
            include uwsgi_params;
            uwsgi_pass unix:///home/pexip/vmrportal/portal.sock;
            access_log /var/log/nginx/portal.access.log;
            error_log /var/log/nginx/portal.error.log;
        }
}
```
