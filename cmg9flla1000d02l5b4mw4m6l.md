---
title: "Transform Old Smartphone in a Digital Audio Player (DAP)"
seoTitle: "Turn Old Smartphone into a DAP"
seoDescription: "Turn an old smartphone into a dedicated Digital Audio Player with apps, tweaks, and ADB removal for an optimal music experience"
datePublished: Thu Oct 02 2025 13:08:44 GMT+0000 (Coordinated Universal Time)
cuid: cmg9flla1000d02l5b4mw4m6l
slug: transform-old-smartphone-in-a-digital-audio-player-dap
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1759090625481/413f676d-a225-4f2a-9dca-1e341eac46d7.png
tags: music, streaming, bash, android, audio, smartphones, adb, shell-script, dac, dap, hi-res, qobuz

---

Watching some videos on turning an old smartphone into a DAP (Digital Audio Player - dedicated), I remembered that I have a Motorola “something-or-other” lying around here.

And this is what I did:

Factory Reset

When logging in (new account, only for the “DAP”), update the apps and OS. It reached the limit of the Motorola One Macro (purchased in 2019 with Android 9):  
upgraded to Android 10 with the latest update from October 2021.

Installed:

**Players**:

* Qobuz
    
* Spotify
    
* USB Audio Player PRO
    
* Poweramp
    
* HiByMusic
    

**For specific headphones:**

* Edifier Connect
    
* Galaxy Wearable (Samsung Gear)
    

**Equalizers**:

* Poweramp Equalizer
    
* Wavelet
    

**Others**:

* Muviz (visual effect)
    
* KDE Connect (multimedia control)
    
* Niagara (launcher)
    
* Keyboard (from “Fossify”)
    

Through Niagara Launcher, I removed all apps from the home screen and configured which music apps (when connecting headphones or a DAC, the players I selected automatically appear on the launcher home screen).

All widgets and bars were removed from the launcher.

BUT...

## ADB

Some notifications from Motorola's bloat apps started popping up... at first, I removed the notification permission, then I installed NetGuard to revoke all access to the Wi-Fi network. However, even so, the apps continue to run and want to show their services.

Next steps:

I enabled developer mode;

I installed adb (Android Debug Bridge) on my Linux laptop;

With the smartphone in dev mode and connected to USB, I continued:

One of the things Wavelet requests is to enable DUMP permission for the app via adb.

I took the opportunity to do this: [https://pittvandewitt.github.io/Wavelet/Settings/](https://pittvandewitt.github.io/Wavelet/Settings/)

But other steps were:

generate a list of all apps installed on the smartphone

```bash
adb -s ZF523242ZG shell pm list packages
```

Note: **ZF523242ZG** is how it identified the smartphone. When connecting to USB, type `adb devices` and it will identify yours.

I moved it to a list:

```bash
adb -s ZF523242ZG shell pm list packages -f > all-packages.txt
```

There were 265 lines!

Obviously, not everything should be removed. I used ChatGPT-5 and Claude 4 Sonnet to generate a new list of what could be removed (safe), aiming to have an Android exclusively for Player (no SIM card, photos, Moto frills, etc.).

This generated `packages-remove.txt` with almost 90 apps listed.

And I removed them with the script below:

### ADB - forcing remove android apps over shell script in Linux

[https://gist.github.com/Esl1h/dd9c1b82bee79e52a27fc346519ee85d](https://gist.github.com/Esl1h/dd9c1b82bee79e52a27fc346519ee85d)

%[https://gist.github.com/Esl1h/dd9c1b82bee79e52a27fc346519ee85d] 

Done!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759090832272/18db637e-9c05-4915-99bc-b78b94013a46.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759090844106/697bdba9-f1a0-4ab7-8b83-b90d29d77c6b.jpeg align="center")

## Connected devices

**The Motorola One Macro has a P2 input, but I'm using this DAC via USB-C**

**Dongle / DAC**

* iFi Go Link – USB DAC (Digital Audio Converter) + Headphone Amp
    
* * ES9219 Sabre DAC chip with Quad DAC+ technology, 32-bit HyperStream III DAC architecture, and jitter eliminator. Supports high-resolution audio up to 32-bit/384 kHz PCM, DSD256, and MQA.
        

**Headphones & Earbuds**

* Samsung Galaxy Buds Pro (1st model) bluetooth
    
* EarbudsAKG K240 Mini MKII - headphones
    
* Edifier W820NB Plus - headset bluetooth
    
* KZ ZSN Pro 2 - IEM
    

Day-to-day: I almost always use the Qobuz app (beta). USB Audio Player PRO and HibyMusic app both are configured with Qobuz account.

And 90% of the time, the iFi Go Link is connected to the AKG K240 Mini MKII.

When I'm away from the studio, I use KZ ZSN Pro 2 (in ear).

I ended up installing and configuring it, but I don't use Bluetooth headphones with it, obviously, as it ends up rendering the USB-C DAC useless, and Bluetooth on this Motorola One Macro is 4.2 (with A2DP/LE), and my actual smartphone uses version 5.3, so using this mobile “DAP” via BT would not be rational.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757365094727/a7548a75-0656-46e6-abc2-4c0cf2b7474b.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757365085188/4037e2a3-ea58-4d6f-97ba-e5cd31f8397a.png?auto=compress,format&format=webp align="left")

## Inspirations

%[https://youtu.be/HuOASuE3lkk] 

%[https://youtu.be/I4DfCmd1vew] 

%[https://youtu.be/WylIWYXrpvQ?si=2TuMKmf2Q6UacB6F]