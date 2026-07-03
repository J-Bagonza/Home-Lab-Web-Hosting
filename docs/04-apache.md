# 04 - Apache

> ← Previous: [03 - Fail2Ban](03-fail2ban.md) | Next: [05 - Cloudflare](05-cloudflare.md)

---

# STEP 4

## nginx

do this if apache isnt installed. i will be using apache on 80.

---

## a) To Install it run this

```bash
sudo apt install nginx
```

---

## b) now enable and start it

```bash
sudo systemctl enable --now nginx
```

---

## c) To verify its running

```bash
systemctl status nginx
```

---

## d) now test it locally on the VM

```bash
curl http://localhost
```

and on windows:

```text
http://192.168.70.17
```

---

You should see nginx's Welcome to nginx page or apache's page if it was already installed on your browser.

---

# STEP 5 For You

## a) Inorder to Remove nginx to avoid future conflicts do this

run these commands:

```bash
sudo systemctl stop nginx
sudo systemctl disable nginx
sudo apt remove nginx nginx-common
```

---

## b) Confirm Apache owns port 80 properly run these

```bash
sudo systemctl enable --now apache2
systemctl status apache2
```

you should see this

```text
Active: active (running)
```

---

## here is why i choose the steps in this order of security first and service second

this order made sense as of the following reasons,

> i deliberately locked down access before installing anything that serves content.
>
> If we'd installed nginx first and opened port 80 before SSH/firewall were hardened,
> there'd have been a window where the box was reachable but under-defended.
>
> Doing security first, service second is the safer order.

---

At this point your server should have:

- SSH secured with key authentication.
- Password authentication disabled.
- A firewall configured.
- Fail2Ban watching SSH.
- Apache serving your website on port 80.

Now its time to make it reachable from the internet.

---

> Next → [05 - Cloudflare](05-cloudflare.md)
