---
title: "Installing Brave Browser on Fedora 41: Easy Workaround"
seoTitle: "Install Brave on Fedora 41: Simple Guide"
seoDescription: "Learn to install Brave Browser on Fedora 41 effortlessly with this simple workaround"
datePublished: Wed Oct 30 2024 21:42:41 GMT+0000 (Coordinated Universal Time)
cuid: cm2wekga7000508l8hcrfdnny
slug: installing-brave-browser-on-fedora-41-easy-workaround
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/4TYi-oGq_EU/upload/7e1c7eb4fb5f9ca25178cbbc5461c14b.jpeg
tags: browser, browsers, fedora, installation, brave, howtos, brave-browser, fedoralinux, fedora41

---

> ***Fix***
> 
> The official page to install Brave Browser on Fedora 41 (dnf5) was updated, follow the link to install:
> 
> %[https://brave.com/linux/#fedora-41-dnf5] 

I installed Brave on Fedora 41 using this workaround until they update the guide on the official page and the repository file.

The dnf config-manager option on this Fedora release was change from `--add-repo` to `addrepo` and it doesn’t recognize the option `autorefresh=1` on .repo file.

Change the second step on [https://brave.com/linux/#fedora-rockyrhel](https://brave.com/linux/#fedora-rockyrhel)

%[https://brave.com/linux/#fedora-rockyrhel] 

Download the file: [https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo](https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo)

There’s only follow lines:

```bash
[brave-browser]
name=Brave Browser
enabled=1
autorefresh=1
baseurl=https://brave-browser-rpm-release.s3.brave.com/$basearch
```

Remove or comment the line: autorefresh=1

run (on the same path that download and edit the file):

`sudo dnf config-manager addrepo --from-repofile=./brave-browser.repo`

And follow the rest of the steps in the guide:

To install asc (GPG Key):

`sudo rpm --import` [`https://brave-browser-rpm-release.s3.brave.com/brave-core.asc`](https://brave-browser-rpm-release.s3.brave.com/brave-core.asc)

And, finally, install Brave Browser on Fedora 41:

`sudo dnf install brave-browser`

I also posted this workaround on the Brave community forum:  
[https://community.brave.com/t/cant-install-brave-on-linux-fedora-41/578115/2?u=eslih](https://community.brave.com/t/cant-install-brave-on-linux-fedora-41/578115/2?u=eslih)

> If you prefer, there's no issue with installing the Flatpak package.
> 
> %[https://flathub.org/apps/com.brave.Browser]