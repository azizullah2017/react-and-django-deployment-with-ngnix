# react-and-django-deployment-with-ngnix

# React
```
cd /home/frontend/
npm run build
```

# Django
```
python3 manage.py runserver_plus 0.0.0.0:5600 --cert-file certifcate.crt --key-file server.key
```


# Ngnix settings

/etc/nginx/sites-available$ sudo nano my_domain.com

```
server {

    listen 80;

    server_name  my_domain.com;

    return 301 https://$server_name$request_uri;

}

server {

      listen 443 ssl;       # Configure the Certificate and Key you got from your CA (e.g. Lets Encrypt)

      ssl_certificate     /home/frontend/certifcate.crt;

      ssl_certificate_key /home/frontend/server.key;       ssl_session_timeout 1d;

      ssl_session_cache shared:SSL:50m;

      ssl_session_tickets off;     # Only use TLS v1.2 as Transport Security Protocol

      ssl_protocols TLSv1.2;     # Only use ciphersuites that are considered modern and secure by Mozilla

      ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';     # Do not let attackers downgrade the ciphersuites in Client Hello

    # Always use server-side offered ciphersuites

      ssl_prefer_server_ciphers on;     # HSTS (ngx_http_headers_module is required) (15768000 seconds = 6 months)

      add_header Strict-Transport-Security max-age=15768000; 

         root /home/frontend/build;
        index index.html index.htm index.nginx-debian.html;         
        
        
        server_name my_domain.com;         
        location / {



                try_files $uri $uri/ =404;


        }
 

}
