# 🐳 Mhira Docker

Docker compose deployment for [MHIRA](https://mhira-project.org) software.

## Pre-requisites

* [Docker](#docker)
* [Docker-compose](#docker)

### Docker

* Docker installation instruction can be found [here](https://docs.docker.com/get-docker/)

* Consider following the post-installation steps e.g. add your user to a docker group.

### Docker-compose

* For Windows and Mac this is installed along with docker. For Linux, installation instructions can be found [here](https://docs.docker.com/compose/install/)

## Installation

Clone this repository to your machine

    git clone https://github.com/mhira-project/mhira-docker.git

Move into the `mhira-docker` folder

    cd mhira-docker

Copy example `environment` file to create your own config

    cp env-example .env

Change the line `JWT_SECRET=changeMe` after the equal sign to set the secret key, which is a 32 character string. Run this line in order to change it into a randomly generated key.

    sed -i "s|changeMe|$(openssl rand -base64 32)|" .env

Run docker-compose to initialize your app

    docker-compose pull
    docker-compose up --build -d

If the above steps ran successfully, you should now be able to access the mhira application on your local machine at below address.

    https://localhost

### External access of MHIRA and HTTPS Certificates

To access MHIRA software from outside of the server set `SITE_DOMAIN` in your environment file to the externally accessible domain name or  IP address of the server.

    # Site domain as domain name
    SITE_DOMAIN=mhira.myorganization.com

    # Or Site domain as server's external IP address
    SITE_DOMAIN=192.168.10.2

    # Also supports multiple domain names or combination of IP address and domains, with comma separation
    SITE_DOMAIN=mhira.myorganization.com, 10.0.0.2

* Restart your docker containers to apply the environment file changes

* Using a publicly accessible domain name as `SITE_DOMAIN`, the bundled in [Caddy](https://caddyserver.com/) webserver will automatically provision a Trusted HTTPS certificate for domain.

* If the using an internal domain name or IP address as `SITE_DOMAIN`, the webserver will provision a self-signed certificate. Users will be faced with a security warning when accessing the site using self-signed certificate, and will need to accept/trust the certificate to access the site.

### HTTP Certificate
## Updating your installation

To update your existing installation to a newer version of MHIRA software execute following commands

    # Pull latest docker config
    git pull 

    # Pull latest docker images
    docker-compose pull  

    # Restart containers with updated images and config   
    docker-compose up -d --build --remove-orphan
