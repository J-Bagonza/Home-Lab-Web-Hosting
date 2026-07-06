# homelab-web-hosting

> my logs into building a web server on a kali linux virtual machine. my host system is windows. by a retard to a retard. retardmaxxing.

i am building the server to be able to host a website and access it from the internet. basically what i am trying to do is replace vercel, and use a vm as my hosting/deployment layer.

this repository is simply my notes. i wrote them while learning, breaking things, fixing them and slowly figuring out how everything fits together. if you're also trying to understand how hosting actually works instead of pressing the deploy button and hoping for the best, maybe these logs will save you a few hours or cost you a few days and some sleep.

---

# what you'll build

by following these logs you'll eventually end up with:

- A hardened SSH configuration using key authentication.
- A properly configured firewall.
- Fail2Ban protecting SSH.
- Apache running on your VM.
- Your server exposed to the internet through a Cloudflare Tunnel.
- Your own deployment layer instead of Vercel.
- A Linux VM capable of hosting static websites and eventually full-stack applications.

---

# documentation

Read the guides in order.

| Step | Guide |
|------|-------|
| 1 | [01 - Server Setup](docs/01-server-setup.md) |
| 2 | [02 - Firewall](docs/02-firewall.md) |
| 3 | [03 - Fail2Ban](docs/03-fail2ban.md) |
| 4 | [04 - Apache](docs/04-apache.md) |
| 5 | [05 - Cloudflare Tunnel](docs/05-cloudflare.md) |
| 6 | [06 - Tailscale](docs/06-tailscale.md) *(coming later)* |
| 7 | [07 - Deployment](docs/07-deployment.md) |
| 8 | [08 - Next.js](docs/08-nextjs.md) *(coming later)* |
| 9 | [09 - Security](docs/09-security.md) *(coming later)* |
| 10 | [10 - Troubleshooting](docs/10-troubleshooting.md) *(coming later)* |

---

# repository structure

```text
homelab-web-hosting/
│
├── README.md
│
└── docs/
    ├── 01-server-setup.md
    ├── 02-firewall.md
    ├── 03-fail2ban.md
    ├── 04-apache.md
    ├── 05-cloudflare.md
    ├── 06-tailscale.md
    ├── 07-deployment.md
    ├── 08-nextjs.md
    ├── 09-security.md
    └── 10-troubleshooting.md
```

---

# things you'll run into

there is a very high chance you'll break something.

that's normal.

i probably broke it first.

---

# disclaimer

these are my personal notes.

i'm documenting what worked for me while building this from scratch.

there are many ways to achieve the same result. this repository documents one of them.

---

happy building.
