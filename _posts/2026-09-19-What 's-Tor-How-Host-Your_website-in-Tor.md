---
title : "What is Tor ,   How to host your Website on Tor"
date : 2026-09-19
categories : [tor]
tags : [fun]

image:
    path:  /assets/img/posts/Host-in-Tor/tor.webp
    alt : Tor
---

Tor is one of the most well-known tools for anonymous browsing. In this article I'll explain where it came from, how it works under the hood, and — as a bonus — how you can publish your very own site on the onion network.

<style>
  .dia { display: block; width: 100%; max-width: 100%; height: auto; margin: 1rem 0; }
  .dia text { font-family: inherit; }
  .dia .box { fill: var(--card-bg, #ffffff); stroke: var(--link-color, #2a80b3); stroke-width: 1.6; }
  .dia .node { fill: var(--highlight-bg-color, #f5f5f5); stroke: var(--main-border-color, #cfcfcf); stroke-width: 1.4; }
  .dia .line { stroke: var(--text-muted-color, #888); stroke-width: 1.6; fill: none; }
  .dia .dash { stroke: var(--text-muted-color, #888); stroke-width: 1.4; fill: none; stroke-dasharray: 6 5; }
  .dia .label { fill: var(--text-color, #34323c); font-size: 13px; text-anchor: middle; font-weight: 600; }
  .dia .sub { fill: var(--text-muted-color, #888); font-size: 10.5px; text-anchor: middle; }
  .dia .cap { fill: var(--link-color, #2a80b3); font-size: 10.5px; text-anchor: middle; font-weight: 700; letter-spacing: .4px; }
  .dia .arw path { fill: var(--text-muted-color, #888); }
</style>

## 1. Origins of Tor

> Onion routing was invented to **separate identification from routing** → **anonymous routing**. Note the wording: it gives you *anonymous routing* — not anonymity. The path you take is hidden, but what you send still has to be interpreted at the other end.
{: .prompt-info }

The concept was born in the mid-1990s at the US Naval Research Laboratory, where researchers designed a way to route traffic so that no single hop could link a user to their destination. Tor itself (the open-source implementation) was released publicly in 2002, and today it is run by a global network of volunteers.

## 2. Understanding Tor

### 2.1 Browsing the Internet the Regular Way

When you open a website the normal way, your request takes a straight, predictable path — and every hop along it can see where you're headed.

<div class="dia-wrap">
<svg class="dia" viewBox="0 0 900 130" role="img" aria-label="HTTP request path without Tor">
  <defs>
    <marker id="arw1" class="arw" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z"/>
    </marker>
  </defs>
  <rect class="box" x="25"  y="45" width="150" height="50" rx="10"/>
  <rect class="box" x="245" y="45" width="150" height="50" rx="10"/>
  <rect class="box" x="465" y="45" width="150" height="50" rx="10"/>
  <rect class="box" x="685" y="45" width="150" height="50" rx="10"/>
  <line class="line" x1="175" y1="70" x2="239" y2="70" marker-end="url(#arw1)"/>
  <line class="line" x1="395" y1="70" x2="459" y2="70" marker-end="url(#arw1)"/>
  <line class="line" x1="615" y1="70" x2="679" y2="70" marker-end="url(#arw1)"/>
  <text class="label" x="100" y="72">You</text>
  <text class="label" x="320" y="72">Wi-Fi Router</text>
  <text class="label" x="540" y="72">ISP</text>
  <text class="label" x="760" y="72">Website</text>
  <text class="sub" x="100" y="90">your computer</text>
  <text class="sub" x="320" y="90">home network</text>
  <text class="sub" x="540" y="90">Vodafone, Orange…</text>
  <text class="sub" x="760" y="90">web server</text>
  <text class="sub" x="207" y="55">1</text>
  <text class="sub" x="427" y="55">2</text>
  <text class="sub" x="647" y="55">3</text>
</svg>
</div>

1. First, your request goes through your **Wi-Fi router**.
2. Your router is connected to your **Internet Service Provider (ISP)**, such as Vodafone or Orange.
3. Your ISP then connects you to the website you wish to visit.

In other words, your ISP knows exactly **which websites** you visit and **when** you visit them.

### 2.2 How a VPN Moves the Problem

A VPN inserts a single extra hop in the middle. Instead of telling your ISP "I'm going to website X", you tell it "I'm going to a VPN server in another country" — a destination that is usually **not blocked** by your ISP. From there, the VPN fetches the page for you.

<div class="dia-wrap">
<svg class="dia" viewBox="0 0 900 145" role="img" aria-label="HTTP request path with a VPN">
  <defs>
    <marker id="arw2" class="arw" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z"/>
    </marker>
  </defs>
  <rect class="box" x="25"  y="45" width="150" height="55" rx="10"/>
  <rect class="box" x="245" y="45" width="150" height="55" rx="10"/>
  <rect class="box" x="465" y="45" width="150" height="55" rx="10"/>
  <rect class="box" x="685" y="45" width="150" height="55" rx="10"/>
  <line class="line" x1="175" y1="72" x2="239" y2="72" marker-end="url(#arw2)"/>
  <line class="line" x1="395" y1="72" x2="459" y2="72" marker-end="url(#arw2)"/>
  <line class="line" x1="615" y1="72" x2="679" y2="72" marker-end="url(#arw2)"/>
  <text class="label" x="100" y="73">You</text>
  <text class="label" x="320" y="73">ISP</text>
  <text class="label" x="540" y="73">VPN server</text>
  <text class="label" x="760" y="73">Website</text>
  <text class="sub" x="320" y="93">sees only traffic to the VPN</text>
  <text class="sub" x="540" y="93">a single point of trust</text>
  <text class="sub" x="540" y="108">IP · credit card · activity</text>
</svg>
</div>

> A VPN simply moves the problem elsewhere: the ISP may no longer see your traffic, but the **VPN operator** replaces it as the single point of trust. Operators can collect personal data — your real IP address, your credit card info, and logs of your online activity.
{: .prompt-warning }

### 2.3 Connecting Through Tor: Onion Routing

Tor works differently: it builds a path through **three independent relays** and encrypts your data three times — like wrapping a letter in three envelopes. Every relay peels off exactly one layer and passes the rest along. Hop 1 knows your IP but not your destination; hop 3 knows the destination but not your IP. **No single relay ever sees both ends.**

<div class="dia-wrap">
<svg class="dia" viewBox="0 0 920 195" role="img" aria-label="UML sequence of onion routing through three relays">
  <defs>
    <marker id="arw3" class="arw" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z"/>
    </marker>
  </defs>
  <rect class="dash" x="185" y="18" width="520" height="140" rx="12"/>
  <text class="cap" x="445" y="38">TOR CIRCUIT — three volunteer relays</text>
  <rect class="box" x="25"  y="78" width="130" height="52" rx="10"/>
  <rect class="box" x="200" y="78" width="130" height="52" rx="10"/>
  <rect class="box" x="380" y="78" width="130" height="52" rx="10"/>
  <rect class="box" x="560" y="78" width="130" height="52" rx="10"/>
  <rect class="box" x="740" y="78" width="130" height="52" rx="10"/>
  <line class="line" x1="155" y1="104" x2="192" y2="104" marker-end="url(#arw3)"/>
  <line class="line" x1="330" y1="104" x2="372" y2="104" marker-end="url(#arw3)"/>
  <line class="line" x1="510" y1="104" x2="552" y2="104" marker-end="url(#arw3)"/>
  <line class="line" x1="690" y1="104" x2="732" y2="104" marker-end="url(#arw3)"/>
  <text class="cap" x="173" y="62">3 layers</text>
  <text class="cap" x="351" y="62">2 layers</text>
  <text class="cap" x="531" y="62">1 layer</text>
  <text class="cap" x="711" y="62">plaintext</text>
  <text class="label" x="90"  y="108">You</text>
  <text class="label" x="265" y="108">Guard</text>
  <text class="label" x="445" y="108">Middle</text>
  <text class="label" x="625" y="108">Exit</text>
  <text class="label" x="805" y="108">Website</text>
  <text class="sub" x="265" y="140">peels layer 3</text>
  <text class="sub" x="445" y="140">peels layer 2</text>
  <text class="sub" x="625" y="140">peels layer 1</text>
</svg>
</div>

> When using Tor, your traffic is routed through **3 servers instead of 1**, as with a VPN. Your traffic is **encrypted 3 times**, Tor servers are run by **volunteers** instead of private corporations, and Tor is **decentralized** — VPNs aren't.
{: .prompt-tip }

That peeling of each layer is exactly why the technique is called **onion routing**.

## 3. A Deeper Look Inside Tor

### 3.1 Relays & the Tor Circuit

Tor relays are volunteer-run servers that simply **relay** (pass) encrypted traffic from one node to the next. Once you pick a circuit, it's reused for a while — then Tor builds a new one with fresh relays.

> Tor is **not** a peer-to-peer (P2P) program like BitTorrent. Your client talks only to the three relays of its circuit — it doesn't exchange data with random users.
{: .prompt-info }

<div class="dia-wrap">
<svg class="dia" viewBox="0 0 900 185" role="img" aria-label="UML diagram of the three relay types forming a circuit">
  <defs>
    <marker id="arw4" class="arw" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z"/>
    </marker>
  </defs>
  <rect class="dash" x="40" y="28" width="800" height="120" rx="12"/>
  <text class="cap" x="440" y="48">A TOR CIRCUIT — one three-hop route</text>
  <rect class="box" x="85"  y="70" width="170" height="60" rx="10"/>
  <rect class="box" x="365" y="70" width="170" height="60" rx="10"/>
  <rect class="box" x="645" y="70" width="170" height="60" rx="10"/>
  <line class="line" x1="255" y1="100" x2="357" y2="100" marker-end="url(#arw4)"/>
  <line class="line" x1="535" y1="100" x2="637" y2="100" marker-end="url(#arw4)"/>
  <line class="dash" x1="815" y1="100" x2="888" y2="100" marker-end="url(#arw4)"/>
  <text class="sub" x="306" y="92">1st hop</text>
  <text class="sub" x="586" y="92">2nd hop</text>
  <text class="sub" x="850" y="92">3rd hop</text>
  <text class="label" x="170" y="103">Entry / Guard</text>
  <text class="label" x="450" y="103">Middle</text>
  <text class="label" x="730" y="103">Exit</text>
</svg>
</div>

- **Entry / Guard relay** — the first hop; it knows who *you* are but not where your traffic is going. It stays in your circuit for a long time, which helps detect malicious relays trying to join the network.
- **Middle relay** — an anonymous middleman; it knows neither you nor your destination.
- **Exit relay** — the last hop; it decrypts the final layer and talks to the target website, so it *does* see the destination.

The full three-hop route is called a **Tor circuit**, and Tor refreshes it periodically to make traffic analysis harder.

### 3.2 Bridges & Pluggable Transports

Governments and ISPs that want to block Tor don't have to block the whole network — they only need to block the **public IP addresses of the guard relays**. Bridges are a workaround: relays that are **not publicly listed**, so censors don't know their addresses.

```
User → Bridge → Tor network → Website
```

<div class="dia-wrap">
<svg class="dia" viewBox="0 0 900 135" role="img" aria-label="Bridges: user connects to an unlisted bridge before Tor">
  <defs>
    <marker id="arw5" class="arw" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z"/>
    </marker>
  </defs>
  <rect class="box" x="25"  y="40" width="150" height="50" rx="10"/>
  <rect class="node" x="245" y="40" width="150" height="50" rx="10"/>
  <rect class="box" x="465" y="40" width="150" height="50" rx="10"/>
  <rect class="box" x="685" y="40" width="150" height="50" rx="10"/>
  <line class="line" x1="175" y1="65" x2="239" y2="65" marker-end="url(#arw5)"/>
  <line class="line" x1="395" y1="65" x2="459" y2="65" marker-end="url(#arw5)"/>
  <line class="line" x1="615" y1="65" x2="679" y2="65" marker-end="url(#arw5)"/>
  <text class="label" x="100" y="67">You</text>
  <text class="label" x="320" y="67">Bridge</text>
  <text class="label" x="540" y="67">Tor network</text>
  <text class="label" x="760" y="67">Website</text>
  <text class="sub" x="320" y="85">a secret, unlisted relay</text>
</svg>
</div>

> You can even request a **private** bridge address that's given only to you — far harder to block than one that's published.
{: .prompt-tip }

To make bridges even harder to spot, most of them use what are called **pluggable transports**. Anyone surveilling your internet traffic might be able to recognize patterns that indicate you're connecting to Tor and block your access to it (though they won't know what you intended to visit over Tor). Pluggable transports prevent this by transforming Tor's traffic to look like something else entirely. The main types on Tor are:

1. **obfs2** — makes your Tor traffic look like random data.
2. **meek** — connects you to the Tor network through a big cloud provider.
3. **snowflake** — routes your connection through Snowflake proxies to make it look like you're placing a video call.
4. **webTunnel** — mimics encrypted web traffic (HTTPS).

---

## 4. Host Your Own Website on the Onion Network

Now for the fun part. Hosting an onion site needs exactly two ingredients: a **web server** that speaks HTTP (Apache or Nginx) and a **Tor service** configured as a hidden service. Nginx only ever listens on `127.0.0.1:80` — the Tor process forwards outside traffic to it.

### 4.1 Install the Requirements

I'll use **Nginx** and **Tor**. Install Tor in your terminal:

```bash
sudo apt install tor -y          # Ubuntu / Debian
sudo dnf install tor -y          # Fedora
sudo systemctl start tor         # start the Tor service
sudo systemctl enable tor        # run Tor automatically on boot
```

Install Nginx:

```bash
sudo apt install nginx -y        # Ubuntu / Debian
sudo dnf install nginx -y        # Fedora
sudo systemctl start nginx       # start the Nginx service
sudo systemctl enable nginx      # run Nginx automatically on boot
```

To make sure everything was installed successfully, check both services:

![Checking the Tor service status](/assets/img/posts/Host-in-Tor/5.png)

![Checking the Nginx service status](/assets/img/posts/Host-in-Tor/6.png)

You can also test Nginx directly: `curl http://127.0.0.1:80` — if you see the welcome page, the installation was a success.

### 4.2 Configure the Hidden Service

Nginx serves files from `/var/www/html/`, so copy your website files into that folder, then point Tor at it:

1. Edit the `torrc` file:
   - `sudo nano /etc/tor/torrc`
2. Scroll all the way to the bottom and add these two lines:
   ```bash
   HiddenServiceDir /var/lib/tor/my_onion_site/
   HiddenServicePort 80 127.0.0.1:80
   ```
3. Restart the Tor service: `sudo systemctl restart tor`
4. Get your .onion address: `sudo cat /var/lib/tor/my_onion_site/hostname`
5. You should see a link like `efknfenfwineifnweifnew.onion` — that's your website's new address.
6. Open the Tor Browser and visit your address.

![Visiting the newly created .onion site](/assets/img/posts/Host-in-Tor/7.png)

Here's the whole architecture in one picture:

<div class="dia-wrap">
<svg class="dia" viewBox="0 0 900 335" role="img" aria-label="UML deployment diagram of hosting an onion site">
  <defs>
    <marker id="arw6" class="arw" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M0,0 L10,5 L0,10 z"/>
    </marker>
  </defs>
  <rect class="node" x="330" y="12" width="240" height="64" rx="10"/>
  <text class="label" x="450" y="38">Tor Browser</text>
  <text class="sub" x="450" y="58">visits your .onion address</text>
  <line class="line" x1="450" y1="76" x2="450" y2="199" marker-end="url(#arw6)"/>
  <text class="sub" x="462" y="120">your onion address</text>
  <text class="sub" x="462" y="133">(kept inside the Tor network)</text>
  <rect class="node" x="80" y="205" width="740" height="115" rx="12"/>
  <text class="label" x="450" y="228">Your server (Ubuntu / Debian / Fedora)</text>
  <rect class="box" x="170" y="250" width="200" height="52" rx="10"/>
  <rect class="box" x="480" y="250" width="270" height="52" rx="10"/>
  <line class="line" x1="370" y1="276" x2="470" y2="276" marker-end="url(#arw6)"/>
  <text class="sub" x="374" y="262">HTTP 127.0.0.1:80</text>
  <text class="label" x="270" y="278">Nginx :80</text>
  <text class="sub" x="270" y="295">serves /var/www/html</text>
  <text class="label" x="615" y="278">Tor hidden service</text>
  <text class="sub" x="615" y="295">onion key + hostname</text>
  <text class="sub" x="450" y="310">/var/lib/tor/my_onion_site/hostname</text>
</svg>
</div>

> Unlike a normal website, an onion service **doesn't need a public IP address** — it's reachable only through the Tor network, which also keeps your server's real location hidden.
{: .prompt-tip }

## Wrapping Up

That's the whole picture: Tor separates **who you are** from **where you're going** by peeling layers of encryption across a three-relay circuit, and with two simple config lines you can put any Nginx site behind a `.onion` address. Whether you want to expose a service without revealing your location, or you just want to experiment, hosting an onion service is a fun and instructive exercise.