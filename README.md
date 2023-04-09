# MultiTasksVPSServer
A docker compose stack to set up several applications on a remote server (VPS, dédicated server asa you have a ssh access and docker installed)

The provided docker-compose is based on docker-swag image to act as a reverse proxy with let's encrypt, and let you expose several applications.
It's a personnal project to déploy atomaticaly with a docker-compose file the following applications:
   - SWAG (and swag dashboard)
      - A nginx gateway which acts as a reverse proxy, a letsencrypt manager, fail2ban protection to safely expose our services 
   - Netdata (to monitor our server)
      - A monitoring app to see what happens on our server
   - Qbittorrent (to give a way to download some holidays movies)
      - an optionnal app if you want to download some files on the bittorrent network
   - Plex (to give a way to play our holidays movies)
      - A media center app you can use to stream your previous downloaded (holidays) movies.
   - Piwigo (if you want to deploy a photos gallery online)
      - if you want to have an online photography gallery
   - Komga (if you want to read some ebooks from your server)
      - if you want to use your server to read ebooks like comics or novels.
   - ownCloud
      - if you want to use your server as your own cloud storage - encrypted - backup needed - special point further! 
