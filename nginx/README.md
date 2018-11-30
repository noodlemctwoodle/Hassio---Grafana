# Install NGINX and Configure Lets Encrypt


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
Once downloaded, you will need to get your certificate, run the following command and follow the on screen prompts
```
./letsencrypt-auto certonly --standalone
```
Once letsencrypt-auto has completed it will display the location of the newly created .pem files. 

Example: 

```
/etc/letsencrypt/live/YOUR_DOMAIN/subdomain.domain.com
```
### Configure NGINX

Change the permissions on the YOUR_DOMAIN folder and it's content by default they can't only be accessed by root
```	
sudo chmod -R 744 /etc/letsencrypt/live/phlex.cleverhomes.io/
```

### Generate strong Diffie Hellman Ephemeral parameter
```
cd /etc/ssl/certs
sudo openssl dhparam -out dhparam.pem 4096
```


## NGINX config
Create a proxy file for NGINX
```
sudo nano /etc/nginx/sites-available/HassioContainers
```
### Edit the NGINX proxy file
Copy [HassioContainers](https://github.com/noodlemctwoodle/Hassio-Containers/blob/master/nginx/config/HassioContainers) into the config file and replace the IP addresses relevant to your netowrk configuration

### Edit NGINX Config
```
sudo nano /etc/nginx/nginx.conf
```
### Edit the NGINX configuration file
Copy the configuration from [nginx.conf](https://github.com/noodlemctwoodle/Hassio-Containers/blob/master/nginx/config/nginx.conf) into the config file replacing all config

### Set proxy File to enabled 
```
sudo ln -s /etc/nginx/sites-available/HassioContainers /etc/nginx/sites-enabled/
```

### Start the NGINX service
```
sudo systemctl start nginx.service
```



## Removing the Proxy file

### Remove Default config from enabled sites
```
sudo rm /etc/nginx/sites-enabled/HassioContainters
```