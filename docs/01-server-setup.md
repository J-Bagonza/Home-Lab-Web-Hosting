Welcome lets start,
STEP 1
``setting up ssh``
-first i set up the ssh after an update so do this 
``sudo apt update
``sudo apt install openssh-server
``sudo systemctl enable --now ssh
``systemctl status ssh

next you need too harden the ssh, here is why , to maintain hygiene , yes hygiene , out on this big internet we have bots and these bots ussually crawl around looking for open ssh ports and try thousands of usernames and passwords automatically, now even if this attack isnt targeted its still pretty bad
but John what about password authentication ? well passwords can be guessed or brute forced no matter how "strong" you think yours is , bots dont get tired. 
but an SSH key pair is different, the private key never leaves your windows machine and its mathematically infeasible to guess.Once you disable passwords , brute force attacks basically become a waterproof sponge they are rendered ineffective.

-----here is how you harden ssh 
a) first to prevent locking yourself out, make sure you have SSH key-based login working,
    ## Generate an SSH key pair
    ssh-keygen -t ed25519
    
   ## to get your VM's IP run this 
      ip a  ##its usually the one that looks like this ``  2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
                                                           inet 192.168.1.15/24 brd 192.168.1.255 scope global eth0 ``
                                                               --here it will be 192.168.1.15

    ##On windows / powershell because my host is a windows os 
    type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh kali@<VM-IP> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
     ``this command copies the public key to the VM before that while running the command it will ask you to create a passphrase , do so for that extra security . 

    ## Test the connection 
    ssh kali@<VM-IP> - enter your passphrase when prompted

-- another thing to check is your windows host ip address to see if they are in the same range as your VM because if they are not your VM wont be reachable to other wider internet devices so make sure your network in the virtual box has not created its own private virtual subnet.If not switch your briged adapter to get on the same network as everything else .

b) Now after the Steps succeed, harden sshd_config
    
    ##on you VM run this 
     sudo nano /etc/ssh/sshd_config
     --go through the file and:( notice that the "replace with" are uncommented use them as is )
                                 Find this:#PermitRootLogin prohibit-password
                                      replace with : PermitRootLogin no
                                 Find this:#PasswordAuthentication yes
                                      replace with : PasswordAuthentication no

c) now restart SSH to apply the changes 
   ``sudo systemctl restart ssh

you can open a new powershell session and try to ssh into your VM , here is what happened: 
SSH used your private key (the secure method) to authenticate, and just asked you to unlock that key locally first.
That's exactly what "key-based auth with a passphrase" is supposed to look like. Password authentication on the VM side is successfully disabled.
