# Nginx
######nginx setup for multiple ports

#####1. Certbot
```
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install certbot
```

#####2. Running Certbot
Note: Nginx will run on these ports.
```
$ sudo ufw allow 80
$ sudo ufw allow 443
$ sudo certbot certonly --standalone --preferred-challenges http -d example.com
```

#####3. Nginx
```
$ sudo apt install nginx
$ sudo ufw app list
$ sudo ufw allow 'Nginx HTTP'
$ sudo ufw status
$ systemctl status nginx
```

#####4. Nginx server setup and get running ports
```

# Example Nginx setup
# both example.com uses port :3011;


server {
        listen 80;
        server_name example.com;
        return 301 https://$host$request_uri;
}

server {
        listen 443 ssl;
        server_name example.com;
        ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
        location /  {
                proxy_pass    http://localhost:3011;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
}


```
