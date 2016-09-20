---
title: Networking Woes
subtitle: The art of setting up Comcast
date: 2016-09-20
bigimg: /img/comcast.png
---

Let me start off by saying unequivocally and without any shadow of a doubt: I am not a networking expert.  Although I understand all the constructs of the web and most all of the requisite information pertaining to how computers and the internet communicate as well as the nuances of local and public networks. 

My real deep protocol knowledge extends mostly into the realm of software development and how bits/bytes are consumed over the wire. I, for one, found it _absolutely fascinating_ how [Microsoft](http://www.microsoft.com) and the [.NET team](http://www.dot.net) built the kestrel http server. There is an awesome video where [Damian Edwards](https://twitter.com/damianedwards) [discusses it here](https://vimeo.com/172009499).

That, however, is a discussion slated for another day. Truly fascinating though how they've basically chosen to look at any given http request like a sequence of `float` values to avoid `string` allocations. 

However, I digress, Today is mainly about 2 things:

1. Venting
2. Helping guide anyone who is running into the same/similar issues with Comcast.
3. Venting

[(Lest we forget one of the hardest problems in Computer Science--Off By One errors)](http://martinfowler.com/bliki/TwoHardThings.html)

## Comcast Default Behavior

So to get on with it: naturally the Comcast modem default behavior is to be extremely confusing. Our public, [static IP](http://www.ipchicken.com) address from [Comcast](http://comcast.com) here is `96.xxx.xxx.9` (blocking some for safety/privacy).  One might expect the Comcast firewall to show this IP with the .10 Gateway, or something similar.  Instead, we get this: 

![Comcast IP](/img/comcast_ip.png)

In case anyone missed that: for some reason I am unable to explain, if you have purchased a static IP or a group of IP's from Comcast your router actually gives off your **Default Gateway** as your _Public IP_ and your Comcast  Business Router will have a _Default Gateway_ referencing some other point or thing on their network (presumably where, in this case, our /30 routes through).

After several visits and phone calls with Comcast, no engineer was able to reliably explain it to me either.  I've moved on to the [acceptance stage of my grief](https://media2.giphy.com/media/hIavT8NX1SE4E/200.gif).

## Setting up your own Router

All that said, this seemingly small thing has an interesting and convenient side effect.  

Since none of the public IP's are in use, any new router you may want to plug in on the inside of it can use _any_ of your available static IP addresses and the Comcast modem is able to stay up and 100% functioning if you so choose (to partition your network, for example).

Now this is where it gets confusing, especially for me--this is because very often we are being dragged into an environment that we are not familiar with, did not set up, and/or there is no one available with the technical information (static IP range, subnet mask, etc) you actually need to set it up.

To make matters worse, you always run the risk of receiving bad information from Comcast should you decide to call for information (Has happened to me, _more than once_!).


### _Sidebar: Which Router?_

As an FYI, the router referenced and used in the specific configuration shown here is a [Mikrotik Cloud Router Switch CRS125-24G](https://www.amazon.com/MikroTik-CRS125-24G-1S-RM-rackmount-enclosure-manageable/dp/B00I4QJSIM) which is also available in a [Non-Rackmount Version](https://routerboard.com/CRS125-24G-1S-IN).

For much more extreme loads, the [Mikrotik Cloud Core Router 1009-8G-1S+](https://www.amazon.com/MikroTik-Cloud-Core-Router-1009-8G-1S-1S/dp/B00KVFLKHG) is recommended.  (_NOTE: This particular version requires much more granular setup & understanding of some networking beyond the scope of this article to complete setup_.)

Once you do have the correct information, here is an example of how the network referenced above is set: 

![Mikrotik Setup](/img/mikrotik-comcast-setup.png)

As you can see and would expect, your _static IP_ and correct [_subnet mask_](https://www.aelius.com/njh/subnet_sheet.html) (in this case, /30) need to be in the WAN IP Address field (this may differ or have a different name in other routers).

The _default gateway_ which the Comcast router uses as its IP, in turn, should be the _Gateway_.

We choose to utilize [Google](http://google.com) and [OpenDNS](http://opendns.com) public DNS servers rather than Comcast's usual defaults of `75.75.75.75` and `75.75.76.76`, though in some cases using the defaults helps to avoid some issues.

## Wrapping It Up

Like I mentioned before; if you follow the strategy outlined above you'll be able to set up your new router/firewall inside of the Comcast network, utilize any one of your static IP addresses as a public, and all the while leave the Comcast Router up & running/functioning perfectly.

For security reasons, I personally turned off just about every single feature & network available on the default Comcast box once I got our [Mikrotik](http://www.mikrotik.com) working properly. This includes turning off all DHCP & Wireless on the Comcast modem.

In my case, I can actually get to `10.1.10.1` (Comcast Router Default IP) while on my other local network of `192.168.88.xxx`.

While convenient, once you have your firewall/router properly configured inside of your Comcast box, you should have very little reason to ever need to log into it.

Your other option is if you want to partition your network into different "areas', you could easily utilize both your Mikrotik Firewall network **AND** your Comcast Firewall network in tandem even if you have only _one_ static IP address.

The Comcast Firewall will broadcast your _Gateway IP_ while your own Firewall will broadcast your assigned _Static IP_.


