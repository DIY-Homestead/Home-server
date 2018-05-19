Setting up nginx proxy
======================

In this guide it is assumed that you have a domain name that points to your server. This could be a top level domain like `example.com` or a subdomain like `homestead.example.com`.

Installing nginx
----------------

```sh
sudo apt install nginx
```

Adding a site
-------------

```sh
sudo vim /etc/nginx/sites-available/homestead.example.com.conf
```

Configuration before adding certificate.

```
server {
	listen 80;
	listen [::]:80;

	server_name homestead.example.com;
}
```

After a certificate is added (see: [Setting up letsencrypt](letsencrypt.md))

```
server {
	listen 80;
	listen [::]:80;

	server_name homestead.example.com;

	return 301 https://$host$request_uri;
}

server {
	listen 443 ssl http2;
	listen [::]:443 ssl http2;

	server_name homestead.example.com;

	ssl_certificate /etc/letsencrypt/live/homestead.example.com/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/homestead.example.com/privkey.pem;

	location / {
		proxy_pass http://servername:80;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Connection "upgrade";
		proxy_set_header Host $host;
	}
}
```

You can add as many location blocks as you like, eg:

```
	location ~ ^/wiki {
		proxy_pass http://servername:80;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Connection "upgrade";
		proxy_set_header Host $host;
	}
```
