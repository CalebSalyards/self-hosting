# Getting Started

# Hardware

## Hardware in-use
This is the easiest, and possibly most convenient thing: using the hardware you already have, even if it's being used for other things. 
This can be problematic in the long term for the following reasons:
* Mantainance: if you need to restart your Windows PC for updates, or if it crashes, so do your services.
* Resource splitting: If something goes wrong in one of your Docker services, say a memory leak, then your Windows side will be impacted
* Security: If someone breaks into your services, it's possible for them to break out of those services and into the rest of your machine.

### Set-up
Assuming you want to use Docker, you can literally just install it on Windows, as it will set up WSL (a small Linux virtual machine) and even add the Docker Desktop app to make it easy to interact with. 
However, this is more dangerous for security. If WSL is compromised, most of the rest of your system is as well. 

You can also install a hypervisor on your PC, such as VirtualBox, VMWare, or Hyper-V, and run a virtual Linux machine on your PC. This will be slower, but it's harder to break out of if you're a bad guy, and you can set harder limits on what resources the VM can utilize.

The final option is installing the services on your PC like any other game or app. 
This is easy, it's also by far the most dangerous for security. For the most part, nobody does this.

## Dedicated Hardware

Get your hands on something, anything. It doesn't have to be very beefy. 
My "dedicated server" was an old family laptop who's WiFi *and* battery had stopped working. 
Poor thing wasn't really a laptop at that point, so I basically got free reign over it. 
Nowadays I have much fancier stuff, but that's honestly not necessary unless you are doing large-scale projects like media servers and backups (as I am currently doing.)

## Cloud Hardware

This is expensive, but only by about $5-15 per month, depending on who you go with.
Basically, cloud services will allow you to effectively rent portions of their powerfull servers and run virtual machines on them. 
This is most commonly referred to as a "compute" resource. 
You can find tutorials, coupon codes, vouchers, and more for lots of these services online. 
Look for things like AWS EC2, Oracle OCI, Google Cloud Compute VM, Azure Compute, and Linode Compute. 
There are more cloud providers than that, but those will get you started if you want to go that way.

There is also a "free tier" with Oracle OCI that you can use. 
It doesn't give you a very powerful machine, but it does give you something. 
All you have to do in theory is give them a credit card (to verify you're not a bot), and you can get a VM for free. 
When I did it, I ended up having to reach out to their support team because it refused to even let me make an account.
Your milage may vary. The only reason I'm using it is because I didn't want to risk an IP address getting found by malicious actors, which is something you can work around for most cases that *aren't* what I was trying to do.

# Software

## Linux (Ubuntu Server)
Windows is fine, and it's what most people are familiar with, but it's resource heavy, and requires a monitor to work on with any degree of convenience.

Linux is far more lightweight, can be far more stable (as in no crashing after long periods of uptime), and as long as you install the right flavor of Linux, it's acually expected that you don't have a monitor installed.

While there are several different server flavors of Linux, I generally recommend using Ubuntu Server, especially if you're just starting out. 
You can just search for Ubuntu Server, download the ISO from their official website, and install it like you do Windows. 
I recommend you install OpenSSH during the setup, and then install Docker after the fact (the pre-installable Docker is installed weirdly and can be problematic). 
If you run into a step where you don't know how to move forward, find a tutorial, do some googling, or ask a GPT service.

## Docker
Docker lets you handle your services far more quickly and easily than any other technology I've played with. Use it, learn it. 
Docker Compose allows you to set up your Docker services by putting all the settings you'll want for it into a file, instead of running it in the command line. 

## SSH
Secure Socket Shell is *the* remote access utility for Linux. 
An SSH client is built into Windows, MacOS and Linux, and possibly even Android here soon. 
If you aren't familiar with SSH, you'll need to be.

# Networking

## Domain
To really do any serious tinkering, I quickly found that I needed a public web domain. 
But, since I'm cheap, I didn't want to actually *pay* for a public web domain, which is something everyone basically expects you to do. 
Most exceptions to this "pay us first" concept have periods of up and down time, because spam exists and scams love using websites. 
You can also implement a VPN feature of sorts if you only plan to use your services yourself, on a limited number of devices.

### EU.ORG
One of the best places imo to get a free site is [eu.org](eu.org). 
It takes a bit of patience, as the site is old and they *do* need to prevent all the bad actors from getting unlimited free domains.
However, if you are persistent, you can use this service to get a domain without paying a penny.

### Tailscale
Tailscale is a VPN service with an extra few benefits. 
One is that you don't need to host your own server: the free tier for Tailscale will let you do basically everything you'll need (unless you're sharing with other users).

The other benefit is something they call MagicDNS. MagicDNS will let you connect to other devices on your 'tailnet' by using their hostname instead of an IP address.

## Reverse Proxy
If you are planning on running more than one service on a machine, you have basically two options:
1. Use a different port for each service, and memorize which port goes to which service.
2. Use a reverse proxy
Reverse proxies can be difficult to set up, but the end result of this is that instead of going to `your-server.org:8443` for `web-server1` and `your-server.org:9443` for `web-server2`, you can instead go to `one.your-server.org` and `two.your-server.org`, using the same port, removing the need for port numbers entirely!

Examples of reverse proxies are:
* Nginx Reverse Proxy
* Caddy
* Traefik

I personally use Traefik because it has mechanisms to easily integrate into Docker Compose, but the others work well too, so I don't want to discount them.

## DNS
DNS is extremely confusing, and can be the bane of everyone's existence for those trying to make a service publicly available. 
The easy way I've found to bypass this problem is with something called Cloudflare Tunnels. 
You need to migrate your domain control to Cloudflare to use it, but once you have done so, you can set up a Cloudflare Tunnel container in your Docker instance, and it will basically tunnel all your web traffic from a publicly accessible "ingress" point into your machine, regardless of where it is. 
This happens to also conveniently bypass the need for port forwarding, so as a student, Cloudflare Tunnels are *very* nice to have.
