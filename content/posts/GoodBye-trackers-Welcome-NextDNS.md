---
title: "GoodBye Ads, Welcome NextDNS!"
date: 2022-02-13T20:07:25+01:00
draft: false
toc: false
images:
tags:
  - DNS
  - privacy
categories:
  - tech
summary: If you love PiHole but love to get it with you without setting up a VPN, then moving to nextDNS might be an option. NextDNS is a PiHole alike cloud DNS adblocker based in the EU.
description: Stop ads and tracking using nextDNS, a kindof PiHole in Cloud.
---

I hear you right there.

I know you've got browser addons like uBlockOrigin, AdGuard and rest. I understand you've even moved to Brave/DuckDuckGo browser to get off Ads & trackers.

**Congratulations!** These are all great ways to achieve the goal to get off trackers & Ads. Not forgetting we're at last homosapiens and are needs are different.

Here I'm going to walk you through my route sharing the experience of the tools I used & ultimately why I'm ready shelling 20 precious Euros from my hard earned money to use NextDNS.

#### Using PiHole and enlightenment

PiHole is lovely! 

Installed it in my homenetowork, delegated it responsibility for DHCP server and Boom! Network-wide adblocking is set. 

How about when not at home? PiHole can be used for DNS queries if I set up a VPN at my home. Unfortunately, upload transfer rate is overly bad at my place. Even though DNS queries are light but I feel the lag. Also, I want my friends and family to use this PiHole but I my experimentation rate using home raspberry pi is so high that I can't break everyone's DNS on performing a stupid change.

#### NextDNS, yes Next! Not sure if it's related to NextCloud in any way.

My understanding of NextDNS is, it's a SaaS version of PiHole. Another beauty is the app, just install it and you need to get your hands dirty with DNS settings. This is so handy for adoption by my family & friends.

#### The No Free Lunch though

> If it's free, consumer is the product.

It's free upto 300,000 queries a month. You can always choose to go with the Pro pack which is €19.90/year. It allows Unlimited queries (ofcourse) along with ability to share it with your close family & friends.

Make sure to explain your family & friends that you can see there logs if you want. I suggest you to turn off logging for the sake of everyone's privacy.

Here are some screenshots from nextDNS UI.

{{< image src="/images/nextDNS/analytics_nextDNS.png"  position="center" style="border-radius: 2px;width:100%;" >}}  
---
{{< image src="/images/nextDNS/analytics_graph_nextDNS.png"  position="center" style="border-radius: 2px;width:100%;" >}}
---
{{< image src="/images/nextDNS/parental_control_nextDNS.png"  position="center" style="border-radius: 2px;width:100%;" >}}
---
{{< image src="/images/nextDNS/security_nextDNS.png"  position="center" style="border-radius: 2px;width:100%;" >}}