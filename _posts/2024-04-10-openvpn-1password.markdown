---
layout: post
title:  "Using OpenVPN with 1password"
date:   2024-04-10 09:38:36 -0400
---

I have a small desktop tower on my local network, an old Dell Optiplex that a friend gifted me. Shout out to Ryan! I run Ubuntu Linux on this machine, with docker containers for Plex, and Homebridge. I also use this server to deploy some hobby projects of my own.

Sometimes I need to start up a VPN tunnel from the Optiplex. Since I already have a ProtonVPN license that I use on the macbook and the iPhone, I thought I should be able to use ProtonVPN on Ubuntu. There's an official package distributed by ProtonVPN, but it's designed for desktop linux. I wanted to be able to run a VPN tunnel connecting to ProtonVPN's servers using the CLI. I looked to see if there a CLI version and there was (v3 Linux CLI) but it didn't work for me. I found that ProtonVPN published a document to [configure OpenVPN for ProtonVPN in Linux](https://protonvpn.com/support/linux-openvpn/). This worked out for me. What I didn't want to settle for is storing the openvpn configuration file, with sensitive information, on the server.
Since I already use 1password, I wanted to store this file in 1password and have some script pipe the file from 1password into the OpenVPN CLI.
I stumbled upon a [project](https://github.com/akinazuki/openvpn-1password) that does exactly this.
As long as you format the 1password entry properly, you will be good. Here is my 1password entry as an example:

![1password entry for OpenVPN to connect to ProtonVPN](/assets/1password-protonvpn.png)

Here's a replay of my shell session:
{% highlight shell %}
$ eval $(op signin)
Enter the password for [redacted] at my.1password.ca:
$ openvpn_1password "op://Personal/ProtonVPNOpenVPN"
...
[2024-04-19T21:20:25Z INFO  openvpn_1password::openvpn] OpenVPN stdout: 2024-04-19 21:20:25 Initialization Sequence Completed
{% endhighlight %}

If you see "Initialization Sequence Completed", you are off to the races!

