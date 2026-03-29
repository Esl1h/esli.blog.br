---
title: "How to get rid of ads everywhere"
datePublished: 2026-03-29T23:02:47.805Z
cuid: cmncd76d7007e1ql4f3q5hlzv
slug: how-to-get-rid-of-ads-everywhere
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/30d1cf66-3ea7-48cb-b90c-d6b3a84938ce.png
tags: ads, youtube, vpn, brave, brave-browser, adblock

---

> TL;DR: Brave Browser + Anti Paywall + NextDNS + VPN + SponsorBlock + SmartTubeNext

The amount of ads on the internet today isn't just annoying, it's a mental health hazard. Every click, every scroll, every video is an obstacle course of banners, pop-ups, autoplaying videos, and sponsored content fighting for your attention like desperate street vendors. Blocking ads isn't just a matter of convenience anymore, it's an act of self-defense. And more than that, it's a statement: this monetization model is broken, intrusive, and unsustainable. The more people block ads, the louder the message gets: the internet needs to find a better way to fund itself without turning every screen into a billboard. Here's how to take back control across all your devices.

## Brave Browser

Desktop and Smartphone: switch to Brave.  
If you don't care about tokens and other built-in features… just disable them. Turn off the VPN button, remove Brave Rewards, and anything else you don't like or that made you dismiss Brave in the past.

Chrome is unbearable - it killed extensions (Manifest V3) and keeps doubling down on tracking. Brave runs on Chromium (**Chromium ≠ Google Chrome**).

With Brave, you can go deeper into ad blocking through filter lists and Scriptlets. But out of the box, Brave Shield already strips away almost everything.

Here are some filters and scriptlets I use to remove specific items on certain sites: [https://github.com/Esl1h/Brave-Filters-and-Scriptlets](https://github.com/Esl1h/Brave-Filters-and-Scriptlets)

Check [https://privacytests.org/](https://privacytests.org/) for privacy data on Brave, in case you still have doubts.

I'm currently keeping an eye on Orion Browser (Kagi) and Ladybird. My second choice is Mullvad Browser (a Firefox fork). But to match what Brave offers out of the box, you'd need to pile on extensions and spend a good while tweaking settings.

### Shields (Brave)

| **Internal URL** | **Purpose** |
| --- | --- |
| brave://shields/ | Global Brave Shields configuration hub, where you can set blocking policies for trackers, ads, and scripts. |
| brave://adblock/ | Manages ad and tracker filter lists, allowing you to add, remove, or customize rules. |
| brave://privacy/ | Dedicated privacy settings page, including permissions, cookies, tracking, and sensitive data controls. |

## Anti-paywall

Here I go into more detail about anti-paywall: [https://esli.blog.br/pulando-o-muro-do-paywall](https://esli.blog.br/pulando-o-muro-do-paywall)

[https://burles.co](https://burles.co) is an extension that blocks the paywall script from running (create to work with Brazilian sites + Tampermonkey).

It can be installed or run via Tampermonkey (install the Tampermonkey extension, enable developer mode, then open the burlesco.js URL and click install).

Another way to block paywalls is through Brave: click on Shields, switch to advanced mode, and block scripts. However, this will block all scripts, not just the ones responsible for the paywall.

## NextDNS

Use NextDNS as your DNS. Even if you already run a Pi-Hole in your home lab, add NextDNS on top of it.

The free tier is limited to 300,000 requests per month, more than enough for regular use. With 4 or 5 devices at home (router + laptops + phones), you'll hardly this queries per month especially with caching enabled. The paid plan costs around €20/year, not that expensive, really.

Link: https://nextdns.io/?from=yes2mwwr

The blocking options, lists, and settings are quite extensive.

And once again, just like Brave Browser, besides removing ads, it also improves your security.

Set up NextDNS on all your devices. Configure it as Private DNS in Brave's settings.

Add NextDNS as Private DNS in your smartphone's advanced settings.

Set it up on your router: this effectively enables ad blocking on devices like Alexa, Smart TVs, and others.

If your phone doesn't allow changing the DNS, consider a different brand next time. But as long as it's configured on the router, you'll still get the blocking benefits when connected to your local network/Wi-Fi.

## YouTube

### VPN

Some countries don't have YouTube monetization, meaning content creators don't earn ad revenue from views in those regions.

By connecting a VPN on your TV or other device to one of these non-monetized countries, you may see no ads at all. Of course, things can get messy — Google might still push ads regardless.

I usually connect through Turkmenistan and Albania — no YouTube ads. Though sometimes YouTube starts showing ads as if I were in France.

Here's a list of countries without monetization: [https://isthischannelmonetized.com/data/youtube-monetized-countries/](https://isthischannelmonetized.com/data/youtube-monetized-countries/)

That said, I've seen some content creators apply geoblocking on their videos.

Using a VPN can block YouTube ads on your smartphone, **but the best approach is to use Brave Browser and ditch the app entirely**.

### Blocking YouTube ads on Smart TVs

My TV is also routed through NextDNS (at the router level), which already blocks Amazon ads on FireStick/FireTV as well as ads on Android TV and Google TV.

On Google TV (updated Android TV), I installed AdGuard, but didn't get the results I wanted. NextDNS on the network plus SmartTubeNext are the best combo to remove YouTube ads on TV and strip out the other ads that show up in the TV's OS or inside other apps.

### SmartTubeNext

For FireTV, Android, and Google TV: [https://smarttubenext.com/](https://smarttubenext.com/)

Install SmartTubeNext. On FireStick, enable developer mode (go to My Fire TV, click the FireStick info several times). Download the Downloader app (there's no way to download it through the default Silk browser on FireStick), then open the SmartTubeNext website inside Downloader, download it, and install.

[https://smarttubenext.com/firestick/](https://smarttubenext.com/firestick/)

It blocks all ads including sponsored segments within videos.

The player and display settings are highly customizable.

If you're not using an Amazon FireTV Stick, use this link for Android TV: [https://smarttubenext.com/android-tv-box/](https://smarttubenext.com/android-tv-box/)

### SponsorBlock

The secret behind SmartTubeNext and other YouTube frontends — whether on Smart TVs or any other device — is SponsorBlock: [https://sponsor.ajay.app/](https://sponsor.ajay.app/)

It's available as a component across many apps that block YouTube ads on Android, Android TV, Google TV, as a Tampermonkey userscript, in players like MPV, Kodi, Chromecast, Roku, Apple TV, iOS, desktop systems (Windows, Linux, and macOS), and finally as a browser extension. Full list here: [https://github.com/ajayyy/SponsorBlock/wiki/3rd-Party-Ports](https://github.com/ajayyy/SponsorBlock/wiki/3rd-Party-Ports)

With SponsorBlock — on any app, any device, any operating system — it will automatically skip video segments containing sponsored content, intro animations, end cards and credits, subscribe/like/comment prompts, paid promotions, and in-video ads.