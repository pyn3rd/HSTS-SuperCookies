# HSTS-SuperCookies



#### Step 1
Create 32 subdomains totally, from 0 to 31, because of the SuperCookie is 32bit.

![image](https://user-images.githubusercontent.com/41412951/125381027-ca7a3080-e3c5-11eb-9775-ecb53ec6c34d.png)


#### Step 2
Update Nginx Server configuration like ``nginx.conf``
```
server {
    server_name  "~^(?<name>\w+)\.hsts\.your-site\.com$";
    listen 80;
    root /var/www/hsts/;

    add_header Cache-Control no-cache;
    set $cors "";

    if ($http_origin ~* (.*\.pynerd.xyz)) {
        set $cors "true";
    }

    location / {
        if ( $https = 'on' ) {
            return 200 'callUpdate($name, 1)';
        }

        if ( $https != 'on' ) {
            return 200 'callUpdate($name, 0)';
        }

        add_header Content-Type application/javascript;
    }

    location /demo {}

    location /set {
        if ( $https = 'on' ) {
            return 200 'ok';
            add_header Content-Type application/javascript;
            add_header Strict-Transport-Security 'max-age=60';
            add_header Access-Control-Allow-Origin '*' always;
            add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS' always;
            add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains' always;

        }
    }

    listen 443 ssl;
    ssl_certificate  /etc/ssl/pynerd.xyz.crt; (or bundle.pem)
    ssl_certificate_key  /etc/ssl/pynerd.xyz.key
}
```

#### Step 3
Upload ``superookies.html`` to your own root directory of Nginx Server 

#### Step 4
Access the page ``https://pynerd.xyz/supercookies.html``, and then fill the blank ``HSTS`` which is the data you plan to send.

![image](https://github.com/pyn3rd/HSTS-SuperCookies/blob/main/supercookie-demo.gif)

#### Step 5

1) Retrieve the response
![image](https://user-images.githubusercontent.com/41412951/125384856-1cbe5000-e3cc-11eb-92b5-61fe5832244b.png)

2) Analyze the result
![image](https://user-images.githubusercontent.com/41412951/125388718-978a6980-e3d2-11eb-813e-10805d75121c.png)


3) The data is ``HSTS``, so you can steal the data which the client-side browser sending from SuperCookie

