# Install Hass.io
This guide explains how to install hassio on Ubuntu Server 18.04 running Docker CE

### Requirements
- docker-ce
- bash
- jq
- curl
- avahi-daemon
- dbus
- Optional
- apparmor-utils
- network-manager
- Run
- Run as root (sudo su):

You may need to install the Universe repo for some of the requirements 
```
sudo add-apt-repository universe
```

### Supported Machine types
Intel-NUC

### Configuration

```
mkdir hassio
cd hassio
git clone https://github.com/home-assistant/hassio-build.git
cd hassio-build/install/
```

### Adjust the hassio_install script to fit your storage locations
```
nano hassio_install
```
Adjust the following lines to set the storage to a location of your choice
```
DATA_SHARE=/usr/share/hassio
```
Set this location to a place of your choice, I have mine on an SSD that is dedicated to the the docker container configurations

EXAMPLE:

```
/config/data/docker/hassio
```

### Make script executable 
```
chmod u+x hassio_install
```

Place the script under /usr/local/bin folder and run the script
```
.\hassio_install
```

### Check Hassio installed correctly 
```
docker ps -a
```

You should see the following containers are up and running 
```
homeassistant/qemux86-64-homeassistant 
homeassistant/amd64-hassio-supervisor
```

## Configure Hass.io for remote access using NGINX reverse proxy
Using the [NGINX Guide](https://github.com/noodlemctwoodle/Hassio-Containers/blob/master/nginx/README.md) create a new certificate for your Hass.io instance. If you have already installed NGINX and Lets Encrypt start from the section 'Create Certificate/s' and generate a new certificate for this Hass.io instance and complete the rest of the guide. 

Be aware that **{config-name}** should be replaced by the NGINX configuration you are using such as 'hassio' or 'containers'




