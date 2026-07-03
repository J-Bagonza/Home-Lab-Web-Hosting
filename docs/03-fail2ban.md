# 03 - Fail2Ban

> ← Previous: [02 - Firewall](02-firewall.md) | Next: [04 - Apache](04-apache.md)

---

# STEP 3

## fail2ban

Putting it as it is, Fail2Ban is an open-source cybersecurity tool that protects servers from brute-force attacks.

Your firewall controls which ports are reachable, but it doesn't care how many times someone tries to log into SSH. Even with password auth disabled, bots will still connect and attempt logins constantly they just fail every time.

That's not a security hole, but it does two things:

- Fills up your auth logs with junk, making it harder to spot anything unusual.
- Uses a small amount of CPU/bandwidth responding to each attempt.

---

fail2ban watches the SSH log, and if an IP fails to authenticate too many times in a short window, it automatically adds a temporary firewall rule blocking that IP entirely.

---

## a) here is how to install it

```bash
sudo apt install fail2ban
```

---

## b) Create a local config override

fail2ban ships with a default config file (`jail.conf`), do not edit that directly, updates will overwrite it.

Instead, copy it to a `.local` file, which fail2ban reads as an override.

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

---

## c) Edit the SSH jail settings

run:

```bash
sudo nano /etc/fail2ban/jail.local
```

look for `[sshd]` and turn it on and set its rules like this

```ini
[sshd]
enabled  = true
port     = ssh
logpath  = %(sshd_log)s
backend  = %(sshd_backend)s
maxretry = 5
bantime  = 1h
findtime = 10m
```

---

## d) Now Enable and start it

```bash
sudo systemctl enable --now fail2ban      # enable
sudo fail2ban-client status sshd          # Verify it's watching SSH
```

---

That's it.

Your firewall decides **who is allowed to knock on the door.**

Fail2Ban decides **who gets kicked off the property after repeatedly trying to break in.**

---

> Next → [04 - Apache](04-apache.md)
