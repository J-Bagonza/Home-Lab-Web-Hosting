STEP 7 The heist
- lets take vercels job .
- I assume you have a website prebuilt like me and you pushed it to github if not do so or clone one of you previous projects and lets move on
  1)On your VM,Install Node.js if not already done , run these commands :
                  ``curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
                  ``sudo apt install -y nodejs
                  ``node -v # checks your node version if you have one
 
  2) Check if Git exists kali usually has it but no harm in checking
                  ``git --version

  3) Clone your repo
                ``git clone https://github.com/J-Bagonza/rs7
                ``cd rs7 
    ----this you can do if you have time -------
      -- say you have  a repo with just index.html and some few other files say images . here is how you will host that website , quick and easy
          3.1) Clear the Apache default using these commands
                   ``sudo rm -f /var/www/html/index.html

          3.2)  Copy your files in
                `` sudo cp -r ~/rs7/* /var/www/html/

          3.3)  Fix ownership so Apache can read/serve them
                 `` sudo chown -R www-data:www-data /var/www/html/
      
          3.4) Verify what's there now
                   ``ls -la /var/www/html/

          3.5) Reload Apache and test locally
                    ``sudo systemctl reload apache2
                    ``curl http://localhost | head -20
  
         3.6) Go public again
                `` cloudflared tunnel --url http://localhost:80


  4) Add your environment variables
                ``nano .env.local

  5) Install dependencies and build
            `` npm install
            `` npm run build

  6) Now we haave to Run it persistently with PM2
  But what is PM2 Linda ? well PM2 is what keeps your application (the Node.js/Next.js process itself) alive. for a static html site , PM2 isn't relevant at all Apache serves static files directly, there's no running process to keep alive. 
  PM2 becomes relevant once you get to the fullstack Next.js stage, where npm start launches an actual server process that needs to stay running continuously.
     commands:      
           ``sudo npm install -g pm2
           ``pm2 start npm --name "myapp" -- start #this tells pm2 to run npm start and keep it running in the background, and label this process myapp so I can refer to it later
           ``pm2 save
           ``pm2 startup   #Generates a command that you need to run that registers PM2 itself with systemd, so that on VM reboot, systemd starts PM2, which then reads that saved snapshot and relaunches myapp automatically no manual intervention needed.

  7) Apache reverse proxy config
    - up to now apaches job was to send back pages , soemeone requests a page , apache finds the matching file and sends it back in baby language serving static content.
    - but now we have and app that runs Nextjs process via PM2 listening on port 3000 apaches job changes entirely, its no longer looking for files its acting as a middleman that takes the incoming requests and quietly relays it to your nextjs app the it relays the apps response back to the browser.
    - this is what we call a reverse proxy ladies and gentlemen.
    - why not just let people hit port 3000 directly ? well your firewall only has 80/443 open not 3000 , apache can handle SSL/HTTPS termination and future features like caching or multiple apps on one server and it keeps a consistent entry point port 80 regardless of whats actually running behind it.
  enough talk here are the commands to do this :
                 `` sudo a2enmod proxy proxy_http   # we are just enabling reverse proxying and ability to proxy httprequests
                 `` sudo systemctl restart apache2  # load the new modules into Apache's running process.
                 `` sudo nano /etc/apache2/sites-available/000-default.conf   # opens Apache's site configuration file rules for how Apache should handle requests on port 80.
                 
         - Add this inside <VirtualHost *:80> 
                         ProxyPreserveHost On                         #this tells it to keep the original hostname the visitor typed instead of replacing it with localhost. this is because Nextjs apps often check the hostname internally without this your app might get confused thinking its being accessed as local host.
                         ProxyPass / http://localhost:3000/           #any request starting from /  send it to whatever's running on localhost:3000 which is your PM2 process
                         ProxyPassReverse / http://localhost:3000/    # Handles the return trip for when your app sends back a redirect or a location header referencing localhost:3000 internally this rewrites it back to look like it came back from apaches address instead thus the browser doesnt get confused seeing an internal address it cant reach

  8) Firing up the quick tunnel
 run these commands 
             ``sudo systemctl reload apache2  #re reads the config file
             ``cloudflared tunnel --url http://localhost:80