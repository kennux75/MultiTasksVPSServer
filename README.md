# MultiTasksVPSServer

A docker compose stack to set up several applications on a remote server (VPS, dédicated server asa you have a ssh access and docker installed)

The provided docker-compose uses several components to act as a reverse proxy with let's encrypt, and let you expose several applications.
It's a personnal project to déploy atomaticaly with a docker-compose file the following applications:
   - SWAG (Secure Web Application Gateway)
      - A nginx gateway which acts as a reverse proxy, a letsencrypt manager, fail2ban protection to safely expose our services
      - Based on: https://hub.docker.com/r/linuxserver/swag
   - Netdata
      - A monitoring app to see what happens on our server
      - Based on: https://learn.netdata.cloud/docs/installation/installation-methods/docker
   - Qbittorrent
      - an optionnal app if you want to download some files or holidays movies on the bittorrent network
      - Based on: https://hub.docker.com/r/linuxserver/qbittorrent
   - Plex
      - A media center app you can use to stream your previous downloaded (holidays) movies.
      - Based on: https://github.com/plexinc/pms-docker
   - Piwigo
      - if you want to have an online photography gallery
      - Based on: https://hub.docker.com/r/linuxserver/piwigo
   - Komga
      - if you want to use your server to read ebooks like comics or novels.
      - Based on: https://komga.org/installation/docker.html
   - ownCloud
      - if you want to use your server as your own cloud storage - encrypted - backup needed - special point further!
      - Based on: https://doc.owncloud.com/server/next/admin_manual/installation/docker/
   - MariaDB
      - MySQL database used to host apps data
      - use the ./mariadb/provision.db.sh file to create your own databases

# Prerequisites:

- Own the domain use for your server - aka <domain.tld>, example.org etc used in this repo. 
- Create an A record for your domain to your server
   " example.org A XXX.XXX.XXX.XXX"
- Create the subdomains you want to use (cloud, plex, www, etc) with CNAME records:
   " www.example.org CNAME example.org."
   " cloud.example.org CNAME example.org"
- .env file
  - There is a .env file provided, used by docker-compose.
  - Modify it to adapt the differents variables as your domain urls, passwords, etc

# USAGE:
   ### STEP1: Set up SWAG
   
   In the docker-compose.yml file, uncomment only the SWAG part.
   We will start SWAG and generate the certificates with the SUBDOMAINS variable.
   Ensure to have <example.org> and all subdomains you want to use already created
      
         # docker-compose up -d
         wait a few seconds...
   
         Check if everything is OK
         # docker logs swag

   At this step, you should be able to see the SWAG welcome page on your domain: https://example.org
   And you should be able to access the SWAG dashboard at the url: https://dashboard.<example.org> (if you have created the domain previously)

   Moreover, the different configuration files have been generated in swag/ directory

   ### Optionnal: protect a subdomain with a .htpasswd:

   If you want to protect the access to this sudomain, you can use the htpasswd command in swag:

      # docker exec -it swag htpasswd -c /config/nginx/.htpasswd anyusername

   And uncomment the htpasswd section in the swag/nginx/proxy-confs/<apps>.<something>.conf file.

   You can reload the nginx configuration by restarting the swag container.

      # docker-compose restart swag


   ### STEP2: Deploy first application and subdomain - netdata
   
   SWAG comes with predefined configuration files to configure nginx as a reverse proxy.

   The configuration files are in swag/nginx/proxy-confs/ directory with the .sample extension.

      # cp swag/nginx/proxy-confs/netdata.subdomain.conf.sample swag/nginx/proxy-confs/netdata.<something>.conf

   Edit it according to your needs.

   In the docker-compose file, uncomment the netdata part, and set everything up.

      # docker-compose down && docker-compose up -d

   You should be able to access netdata at the url you've configured, for instance: netdata.example.org

   If not, check that netdata has correctly started, or check nginx logs.

   The nginx logs are in swag/logs/nginx/ directory.


   ### STEP3: Deploy the applications you want
   
   Cp the necessary files in swag/nginx/proxy-confs/<apps>.sample to swag/nginx/proxy-confs/<apps>.conf.
   Uncomment the corresponding part in the docker-compose file, and restart with a docker-compose down and up.
   
   - Qbittorrent
   
   No particular changes for qbittorrent. At the first connection, you should be able to connect with the default admin password.
      
   - Mariadb: for applications which need a mysql database:
   
   Uncomment the mariadb part in docker-compose file.

   In the mariadb/init/ directory, modify the provision.sql file to create the needed databases and users at the first start.
   Uncomment the application part in docker-compose (piwigo, owncloud,...) and change the environment variables to setup database access.

   - Plex:

   Prerequisites to deploy the Plex applications:

    - Own a plex account on plex.tv
   
    - Generate a plex claim token (valid for 4minutes) at https://plex.tv/claim/ and set PLEX_CLAIM environment variable.

   The volume used by Plex is the same one where qbittorrent puts the downloaded files.

   - Komga:

   Komga uses a subdirectory of qbittorrent as well qbittorrent/downloads/ebooks

   When you use qbittorrent to download your ebooks, or if you want to read your own ebooks, place them in this directory

   - OwnCloud:

   The owncloud part in docker-compose.yml is a lightly modified of https://raw.githubusercontent.com/owncloud/docs-server/master/modules/admin_manual/examples/installation/docker/docker-compose.yml

   Uncomment the owncloud part, and the redis part as well, it's a dependency of owncloud.

   Otherwise, follow instructions at https://doc.owncloud.com/server/next/admin_manual/installation/docker/, setting the variables OWNCLOUD_DOMAIN and OWNCLOUD_TRUSTED_DOMAINS.


At the end.. Back it UP!

Still a part to add to integrate KOPIA, a backup solution.
