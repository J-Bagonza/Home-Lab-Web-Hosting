STEP 2
``Setting up the firewall on your VM `` 
something you have to know before you do this is that Linux has a built-in firewall system called netfilter in the kernel, but configuring it directly (via iptables or nftables) easy to get wrong — one misplaced rule and you can lock yourself out or leave a hole open tricky situation to find yourself in. 
ufw "Uncomplicated Firewall" is a friendly wrapper around that same kernel firewall — same protection, much simpler syntax for us .

a) to Install it run 
   ``sudo apt install ufw

b)set the default policy — deny everything incoming
  - why does this matter ?
    well, without this, any service you install in the future (or already have running that you forgot about) is reachable by default.
    Default-deny flips that — nothing gets in unless you explicitly say so.
    Outgoing stays allowed since your VM itself needs to reach the internet (updates, DNS, etc.) without restriction

 to apply run these commands 
  `` sudo ufw default deny incoming
     sudo ufw default allow outgoing ``

c) Allow SSH by running this command
 ``sudo ufw allow ssh

d) To Allow web traffic Run these commands
  ``sudo ufw allow http
    sudo ufw allow https``
  - This opens port 80 (nginx, plain HTTP) and 443 (HTTPS, once certbot is set up later)

e) now Enable ufw
  ``sudo ufw enable

f) To Verify 
  `` sudo ufw status verbose
- you should see this 
   Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
443                        ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
443 (v6)                   ALLOW IN    Anywhere (v6)      

- now that should confirm that SSH,HTTP,HTTPS are open and everything else denied by default and both IPV4 AND V6 are covered.

