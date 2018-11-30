# Install NGINX and Configure Lets Encrypt

Be aware that {config-name} ussed in this guide should be replaced by the NGINX configuration you are using such as 'hassio' or 'containers'


### Install NGINX 
```bash
sudo apt-get install nginx
```
### Make NGINX run as boot
```
sudo update-rc.d nginx defaults
```
### Install Lets Encrypt
```
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt/
./letsencrypt-auto --server \ https://acme-v01.api.letsencrypt.org/directory --help
```

### Create Certificate/s
Once downloaded, you will need to get your certificate, run the following command and follow the on screen prompts
```
./letsencrypt-auto certonly --standalone
```
Once letsencrypt-auto has completed it will display the location of the newly created .pem files. Make a note of this location as you will need it later for the proxy configuration

Example: 

```
/etc/letsencrypt/live/YOUR_DOMAIN/hassio.mysmarthome.co.uk
```
### Configure NGINX

Change the permissions on the YOUR_DOMAIN folder and it's content by default they can't only be accessed by root
```	
sudo chmod -R 744 /etc/letsencrypt/live/hassio.mysmarthome.co.uk/
```

### Generate strong Diffie Hellman Ephemeral parameter
```
cd /etc/ssl/certs
sudo openssl dhparam -out dhparam.pem 4096
```


## NGINX config
Create a proxy file for NGINX
```
sudo nano /etc/nginx/sites-available/{config-name}
```
### Edit the NGINX proxy file
Copy [HassioContainers](https://github.com/noodlemctwoodle/Hassio-Containers/blob/master/nginx/config/HassioContainers) into the config file and replace details below to reflect your netowrk configuration. You can comment out or remove any 'proxy_pass' script blocks you are not going to use

Here are some of the examples you will need to change


Change Domain Name Example:

```
server {
    listen                    80;
    listen                    [::]:80;
    server_name               hassio.mysmarthome.co.uk; # Change the domain name here
    return                    301 https://$server_name$request_uri;
}

server {
    listen                    443 ssl http2;
    listen                    [::]:443 ssl http2;
    server_name               hassio.mysmarthome.co.uk; # Change the domain name here
```

Change the certificate example:

```
    ssl_certificate           /etc/letsencrypt/live/hassio.mysmarthome.co.uk/fullchain.pem;
    ssl_certificate_key       /etc/letsencrypt/live/hassio.mysmarthome.co.uk/privkey.pem;
    ssl_trusted_certificate   /etc/letsencrypt/live/hassio.mysmarthome.co.uk/chain.pem;
```




Change IP Detials Example:

```
    location ^~ /tautulli {
        proxy_pass            http://10.20.30.199:8181/tautulli;
                include               proxy_params;
    }
```
Comment Out Example:

```
#    location ^~ /tautulli {
#        proxy_pass            http://192.168.0.10:8181/tautulli;
#                include               proxy_params;
#    }
```



### Edit NGINX Config
```
sudo nano /etc/nginx/nginx.conf
```
### Edit the NGINX configuration file
Copy the configuration from [nginx.conf](https://github.com/noodlemctwoodle/Hassio-Containers/blob/master/nginx/config/nginx.conf) into the config file replacing all config

### Set proxy File to enabled 
```
sudo ln -s /etc/nginx/sites-available/{config-name}/etc/nginx/sites-enabled/
```

### Start the NGINX service
```
sudo systemctl start nginx.service
```



## Removing the Proxy file

### Remove Default config from enabled sites
```
sudo rm /etc/nginx/sites-enabled/{config-name}
```