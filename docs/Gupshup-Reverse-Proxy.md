---
id: gupshupReverseProxy
title: Gupshup Reverse Proxy
sidebar_label: Gupshup Reverse Proxy
---

## 1. Overview

Gupshup is the service provider that connects our services to the messageing channels like whatsapp etc. Gupshup receives messages from user & sends it to our service for further processing and vice versa. 

We should create a reverse proxy for gupshup to send the messages to, it receives from the channels. 

Gupshup will provide a number which will be used to perform communication between the user & bot, it will send the messages to the proxy associated with the number.

## 2. How to create a proxy

1. Login to your server.

2. Create a nginx sites-available config file, if it does not exists.

	```nano /etc/nginx/sites-available/default```

3. Add the following code for the newly created config file.

	```
	server {
	        listen 80 default_server;
	        listen [::]:80 default_server;

	        root /var/www/html;
	        index index.html index.htm index.nginx-debian.html;

	        server_name _;
	    }
	```

4. Add below code in the enclosed brackets.
	```
		location /gupshup/whatsApp  {
		    proxy_pass http://ip:8080/gupshup/whatsApp; // Eg. http://143.1.1.125:8080/gupshup/whatsApp;
		}
	```

	After adding the above lines, the file should look like this. 
	```
	server {
		listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _;

		location /gupshup/whatsApp  {
		    proxy_pass http://ip:8080/gupshup/whatsApp; // Eg. http://143.1.1.125:8080/gupshup/whatsApp;
		}
	}
	```

5. Contact the Gupshup support team to add below url for a specific number, which will be later used to communicate for bot conversations.

	```https://host/gupshup/whatsApp //Eg. http://www.example.com/gupshup/whatsApp```
