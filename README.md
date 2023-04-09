# MultiTasksVPSServer
A docker compose stack to set up several applications on a remote server (VPS, dédicated server asa you have a ssh access and docker installed)

The provided docker-compose uses several components to act as a reverse proxy with let's encrypt, and let you expose several applications.
It's a personnal project to déploy atomaticaly with a docker-compose file the following applications:
   - SWAG (and swag dashboard)
      - A nginx gateway which acts as a reverse proxy, a letsencrypt manager, fail2ban protection to safely expose our services
      - Based on: https://hub.docker.com/r/linuxserver/swag
   - Netdata (to monitor our server)
      - A monitoring app to see what happens on our server
      - Based on: https://learn.netdata.cloud/docs/installation/installation-methods/docker
   - Qbittorrent (to give a way to download some holidays movies)
      - an optionnal app if you want to download some files on the bittorrent network
      - Based on: https://hub.docker.com/r/linuxserver/qbittorrent
   - Plex (to give a way to play our holidays movies)
      - A media center app you can use to stream your previous downloaded (holidays) movies.
      - Based on: https://github.com/plexinc/pms-docker
   - Piwigo (if you want to deploy a photos gallery online)
      - if you want to have an online photography gallery
      - Based on: https://hub.docker.com/r/linuxserver/piwigo
   - Komga (if you want to read some ebooks from your server)
      - if you want to use your server to read ebooks like comics or novels.
      - Based on: https://komga.org/installation/docker.html
   - ownCloud
      - if you want to use your server as your own cloud storage - encrypted - backup needed - special point further!
      - Based on: https://doc.owncloud.com/server/next/admin_manual/installation/docker/


# Prerequisites:
- Own the domain use for your server - aka <domain.tld>, example.org etc used in this repo. 
- Create an A record for your domain to your server
   " example.org A XXX.XXX.XXX.XXX"
- Create the subdomains you want to use (cloud, plex, www, etc) with CNAME records:
   " www.example.org CNAME example.org."
   " cloud.example.org CNAME example.org"

# USAGE:
   STEP1:
   
   We will start SWAG and generate the certificates with the SUBDOMAINS variable.
   Ensure to have <example.org> and all subdomains you want to use already created
      
         # docker-compose up -d
         wait a few seconds...
   
   At this step, you should be able to see the SWAG welcome page on your domain: https://example.org
   And you should be able to access the SWAG dashboard at the url: https://dashboard.<example.org> (if you have created the domain previously)

   STEP2:
   
      "plop":
         test
