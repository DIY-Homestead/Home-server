Setting up letsencrypt
======================

Installing
----------

```sh
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
```

Basic usage
-----------

Create a new certificate for a site already configured in nginx.

```sh
sudo certbot --nginx certonly
```

The first time you will be asked for your credentials including email. When the certificate is close to expiery, you will be notified by email. To renew a speciffic sertificate simply use

```sh
sudo certbot --nginx certonly -d homestead.example.com
sudo service nginx reload
```

If you want to fully automate this, check that all configuration is ok

```
sudo certbot renew --dry-run
```

And then simply add a script to you your cron daily

```
sudo certbot renew
```
