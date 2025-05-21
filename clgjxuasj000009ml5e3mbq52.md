---
title: "Which KeePass version is best for me?"
seoTitle: "Best KeePass Version: Choosing the Right One"
seoDescription: "Explore the ideal KeePass version for you, comparing features, compatibility, and security across KeePass, KeePass2, KeePassX, KeePassXC, and more"
datePublished: Sun Apr 16 2023 21:47:41 GMT+0000 (Coordinated Universal Time)
cuid: clgjxuasj000009ml5e3mbq52
slug: which-keepass-version-is-best-for-me
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/eNxYF6cexYU/upload/414830469ada14a7c1b6f79ee75ae394.jpeg
tags: linux, security, passwords, keepass, keepassxc

---

Keepass, Keepass2, KeepassX, KeepassXC and others... What is the difference?

KeePass is a free and open-source password manager which helps you manage your passwords securely. It is stored in a database, which is locked with a master key and fully encrypted.

I realize that as you read this article, you're likely familiar with password managers and have some knowledge of KeePass or other popular systems such as LastPass, Bitwarden, Dashlane, and 1Password. While all of these options are online and offer limited features in their free versions, they also sell subscription plans. This makes KeePass an excellent alternative in various situations.

* ## KeePass
    

The official KeePass is [https://keepass.info/](https://keepass.info/), with the initial release in 2003! The newest versions are 2.53 and 1.41 (when I wrote this article), released in January 2023 (less than 5 months after the previous release).

On the site, you will find 2 versions available: KeePass 1.x and KeePass 2.x One is not the evolution of the other! They are developed in parallel, and both are free and open source. However, 1.x uses an older platform and is still supported (2.x is not!).

Both versions use AES 256, with KeePass 2.x through plugins able to add and support other algorithms.

The differences:

### KeePass 1.x

Written in C++, runs only on Windows platforms (or using Wine on Linux). The password database has the extension KDB. In addition to AES 256, there is Twofish 256 bits and data authentication/integrity is guaranteed using a plain text SHA-256 hash.

### KeePass 2.x

Introduced in 2007, KeePass 2.x offers more support and features than KeePass 1.x, including OTP (One Time Password) support, Smart Cards, an extensive list of plugins, and remote database support. Developed in C# using .NET, it can be run on Linux with Mono. The password database uses the KDBX extension. Besides AES 256, it supports ChaCha20 256-bit encryption, and data authentication/integrity is ensured through an HMAC-SHA-256 ciphertext hash.

In the database versions, there are also changes while KDBX3 uses AES-KDF KDBX4 (most current) uses Argon2.

In this link, there is a developer's table with the comparison between versions: [https://keepass.info/compare.html](https://keepass.info/compare.html)

Here is the list of plugins (with information on which version they support, 1x or 2x): [https://keepass.info/plugins.html](https://keepass.info/plugins.html)

Here you can find more details about security differences between versions: [https://keepass.info/help/base/security.html](https://keepass.info/help/base/security.html)

## Forks and Ports KeePass-Related Projects

* ### KeePassL
    

A fork of KeePass to run on Linux (when there was no Mono and remembering that KeePass 2.x came out in 2007!).

* ### KeePassX
    

When KeePass L became cross-platform in 2006, it changed its name to KeePassX. Cross-platform means Mac OS X, Windows and the tarball for Linux. Some third party developers created packages for distros.

KeePassX was officially discontinued in December 2021, with its final release dating back to 2016.

* ### KeePassXC
    

KeePassXC was introduced in 2012 as a fork of KeePassX due to its extensive development process. Essentially, it continues to serve as a multi-platform port of KeePass 2.x, but is built using C++, eliminating the need for Mono installation on Linux.

It's compatible with both KDBX3 and KDBX4 formats, although the goal is to transition to KDBX4. Additionally, it can import KeePass 1.x (KDB) files, but does not support KeePass 2.x plugins.

It officially has browser extensions! For the others, you need to look for some third-party extensions.

* ### KeeWeb
    

KeeWeb is a KeePass-compatible fork developed in JavaScript, designed to be multi-platform and function as a web app. Users can access their database either through a browser, with the option to upload their web app instance, or via a client installed on their operating system (Electron). Launched in 2016, KeeWeb is the newest addition to this list, with its latest stable version released in 07/2021. However, the project's GitHub currently lacks a dedicated maintainer.

It is possible, using KeeWeb, to point to the password database directly stored in some provider (like Google Drive, Dropbox, OneDrive or any other that supports webDAV). While the others need to be done another way, like mapping the remote file to the operating system, either by mounting a path (webDAV, samba, NFS, FTP...) or by other means (like Dropbox's client), but for Google Drive, OneDrive and some others, just downloading the file (or using some third-party app like OneDriver or InSync).

For browsers, the project site indicates using the KeePassXC extension.

* ### AuthPass
    

Similar to KeeWeb, there is AuthPass. Its latest version is from 06/2022 and it seems that its development is more active. Made in Flutter, and multi-platform (including mobile).

* ### KPCLI
    

Kpcli is a command-line interface (CLI) client compatible with both KeePass 1 and 2 databases (KDB and KDBX family). With the help of bash, you can create scripts to automate password usage via the command line, eliminating the need for a graphical client or KeePass itself.

## Other ports/forks

On the KeePass site, there is a list: [https://keepass.info/download.html](https://keepass.info/download.html)

However, on GitHub, there is an even longer list, separated by platform-specific clients, cross-platform clients, mobiles, plugins and tools. [https://github.com/lgg/awesome-keepass](https://github.com/lgg/awesome-keepass)

## Conclusion

Let me help you decide which version of KeePass is best for you.

Do you use Windows? Give KeePass2 a try. If you're not interested in KeePass 2.x, no worries! Just refer to the list above and pick any alternative that suits you. ;-)

Are you a Linux user? Give KeePassXC a try.

Concerned about usability and compatibility across multiple operating systems? Try KeePassXC on Linux, Windows, or Mac for a seamless experience.

Looking to use it in your browser, just like those paid/freemium competitors? Give KeePassXC a shot and simply install the official extension.

Looking to access it on Android as well? Give KeePassDX a try. It's compatible with all password database versions (KDB, KDBX...), ensuring seamless integration with any desktop app you choose. If you opt for AuthPass, it's the only one with an official Android version. For all other options, you'll need to depend on apps developed by third parties.

> **Moreover, implement 2FA/MFA for every feasible application!**
> 
> When faced with a choice between two services or websites, opt for the one that provides 2FA/MFA (ideally through an app rather than SMS).