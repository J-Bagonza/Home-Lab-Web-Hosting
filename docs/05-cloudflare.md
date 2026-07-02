STEP6 Port Fowarding
- for this step it usually involves you having a router but if you dont have one you could skip port forwarding entirely, and use a tunnel with some demerits.
- first i will show how you use a tunnel 
  A tunnel allows your VM to make an outbound connection to a public relay service, and that relay forwards internet traffic to your VM through that connection. Outbound connections are essentially always allowed, so you don't need any router access at all.
  there are several options you can install and use for this such as tailscale but i will show both in use starting with Cloudflare Tunnel. no inbound ports, no router config, no public IP needed.
-lets get started 

 Setting up cloudflare ( its free of course) - but you need to buy a domain for ease i will explain
   1)  To Install cloudflared run this commands one by one 
        `` Sudo curl -L https://pkg.cloudflare.com/cloudflare-main.gpg -o /usr/share/keyrings/cloudflare-main.gpg
           echo 'deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared jammy main' | sudo tee /etc/apt/sources.list.d/cloudflared.list
           sudo apt update
           sudo apt install cloudflared ``

    - these commands do this a) Download Cloudflare's GPG key so that Linux package managers verify packages before installing them.
                             b) Add Cloudflare's repository telling kali that there is another software repository you should check for packages.
                             c) that just Updates package lists
                             d) also there is no typo in sudo apt install cloudflared , cloudflared is actually the command line client that allows your machine communicate securely with Cloudflare services.

  2) Do a Quick test tunnel this will get you a temporary public URL , good for confirming everything works before committing further. run this command :
       ``cloudflared tunnel --url http://localhost:80

         you will see a link like such " https://introduce-minnesota-action-involve.trycloudflare.com " visit it and you will see the default apache page.

- so now you have a cloudflare quick tunnel url publicly reacheable and costs nothing now to understand me , the domain isn't a technical requirement to "make it work" it's about what kind of thing you're building.
- a domain of around 0.98 usd at namecheap will give you this , a permanent address unlike the random one geenrated by cloudflare, independence from any single provider, and i am building this to host a fullstack website so i will and you too will need it later on. it also gives you SEO / discoverability.

-here is my plan going foward showing you the various ways you can do this mostly if you are Mr crabs 
    a) for Mr Crabs sincerely from Spongebob.
- so right now we have Deployed and secured a self-hosted Apache web server on a Linux VM, exposed to the internet via Cloudflare Tunnel, with hardened SSH, firewall rules, and intrusion prevention.
- to remind you our goal was to replace vercel the hosting/deployment layer with our own VM but we still keep other 3rd part services like supabase, redis resend etc . So lets see how we will deploy a full website from scratch on our own Linux VM
