---
title: "WireGuard"
datePublished: Tue Sep 07 2021 16:24:01 GMT+0000 (Coordinated Universal Time)
cuid: cktaa8vs0040h7ts17em10yri
slug: wireguard
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1631031655407/8EQ7rzpvFl.png
tags: linux, server, servers, security, linux-kernel

---

What is WireGuard?

WireGuard is an easy-to-configure, fast, and secure, open-source layer 3 tunnel (VPN) that uses state-of-the-art encryption. Its goal is to provide a faster, simpler, and easily deployable general purpose VPN.

WireGuard aims to replace IPsec in most use cases and TLS based solutions like OpenVPN.

Most other solutions like IPsec and OpenVPN were developed decades ago. Security researcher and kernel developer Jason Donenfeld realized that they were slow and difficult to properly configure and manage. This made him create a new open source VPN protocol and solution that is faster, more secure, and easier to deploy and manage.

WireGuard uses Curve25519 for key exchange, ChaCha20 for encryption and Poly1305 for data authentication, SipHash for hashtable keys and BLAKE2s for hash. The ChaCha20-Poly1305 defaults to IPsec and OpenVPN (via TLS).

WireGuard was originally developed for Linux, but is now available for Windows, macOS, BSD, iOS and Android. It's still under heavy development.

Another good thing about WireGuard is that it has a lean code base with only 4000 lines of code. Compared to OpenVPN which reaches 400,000 lines of code, it is clearly easier to debug.

Since WireGuard runs in kernel space, it provides secure networking at high speed.

It has been merged into Linux Kernel 5.6 and currently, you can install WireGuard on Linux as a kernel module (DKMS)

When you install WireGuard as a kernel module, you basically modify the Linux kernel on your own and add some code to it. Starting with the 5.6 kernel, you don't need to manually add the kernel module. It will be included in the kernel by default.

Including WireGuard in Kernel 5.6 will likely extend WireGuard adoption and therefore change the current VPN landscape.

WireGuard is gaining in popularity for good reason. Some of the popular privacy-oriented VPNs, such as Mullvad VPN, are already using WireGuard and adoption is likely to grow in the near future.

Although WireGuard is currently at version 1.0.0 on Linux, its Windows package is at beta version at 0.1.0; it has added significant performance, stability, localization and accessibility features since the step-by-step preview of an older version.

Mac and BSD users do not yet have a kernel option for WireGuard support, but can run a Go language implementation from their respective repositories - pkg install wireguard on FreeBSD and brew install wireguard-tools, port install wireguard- tools, or even in the Apple Store on the Mac.

iOS users can find WireGuard in the App Store, and Android users can find it in the Play Store.

*It's not all flowers*, see this article called "Why Not Use Wireguard" from the IPFire blog: blog.ipfire.org/post/why-not-wireguard

It lists some items like:

- Do not accept dynamic IP on Server
- Not being able to change the server without having to update clients

In this Linode article, walkthrough to configure WireGuard (Client and Server) on Ubuntu: https://www.linode.com/docs/networking/vpn/set-up-wireguard-vpn-on-ubuntu/

Below the creator's PDF, showing how Wireguard works: https://wireguard.com/papers/wireguard.pdf

Official website: https://wireguard.com

Installation packages: https://wireguard.com/install