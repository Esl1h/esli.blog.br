---
title: "Ubuntu doesn't start graphic UI with Ryzen 3000 series"
datePublished: Tue Sep 07 2021 16:12:28 GMT+0000 (Coordinated Universal Time)
cuid: ckta9u0ii03tu7ts1am7r315v
slug: ubuntu-doesnt-start-graphic-ui-with-ryzen-3000-series
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1631031011531/BdhfHxxYQ.jpeg
tags: help, ubuntu, linux, hardware, linux-for-beginners

---

Not starting graphical mode is a symptom of many issues. 
Below I describe one of them, which can be summarised as: 
- Ubuntu 20.10 with AMD Ryzen CPU does not start graphics mode after upgrading or installing new hardware. 
- Ubuntu 20.10 with AMD Ryzen CPU does not start graphics mode after installing SSD or NVMe.
- Ubuntu 20.10 with AMD Ryzen CPU and RADEON GPU does not start graphics mode after installing new hardware. 

But this problem can occur with any Linux distribution because (at the end of this text), it isn't related to Linux directly. 

My Environment: 

CPU Ryzen 9 3900x 

Motherboard AsRock Master X470 SLI/ac 


I made two hardware changes: 
I added an ASUS RADEON RX5600 XT and replaced a HyperX SSD with an NVMe Western Digital. 

In the exchange of SSDs, I changed my Linux distro, moving from Ubuntu 20.04 to Ubuntu 20.10 (Ubuntu Studio in both, and until 20.04, it was XFCE, and from 20.10 it was replaced by KDE Plasma). 

After installation, configuration and some reboots... 

The graphical mode stopped starting; another symptom was when it was idle for a long time, it didn't go into screen lock (it was in black screen), but it didn't go back to graphical mode. 

Some random failures (ok, I didn't investigate deeply in this case), where it didn't start the keyboard or connect to the network.

Serious bug in Ubuntu 20.10? 
Defect in nvme? 
Linux bug with NVMe m.2? 
Linux bug with Radeon card or card defect? 

In one of the errors, when not starting graphical mode, I used CTRL + ALT + F1 to access the console, login with the user and run `startx`.


```
root@linux:~# systemctl status sddm.service  

● sddm.service - Simple Desktop Display Manager
    Loaded: loaded (/lib/systemd/system/sddm.service; enabled; vendor preset: enabled)
    Active: active (running) since Wed 2020-11-04 08:12:42 -03; 6h ago
      Docs: man:sddm(1)
            man:sddm.conf(5)
  Main PID: 1546 (sddm)
     Tasks: 2 (limit: 76974)
    Memory: 11.1M
    CGroup: /system.slice/sddm.service
            └─1546 /usr/bin/sddm

nov 04 08:12:42 Linux systemd[1]: Starting Simple Desktop Display Manager...
nov 04 08:12:42 Linux systemd[1]: Started Simple Desktop Display Manager.
nov 04 08:12:42 Linux sddm[1546]: WARNING: CPU random generator seem to be failing, disable hardware random number generation
nov 04 08:12:42 Linux sddm[1546]: WARNING: RDRND generated: 0xffffffff 0xffffffff 0xffffffff 0xffffffff
nov 04 08:12:42 Linux sddm[1546]: Initializing...
nov 04 08:12:42 Linux sddm[1546]: Starting...
nov 04 08:12:42 Linux sddm[1546]: Logind interface found

``` 

The display manager used in this Ubuntu release is SDDM (Simple Desktop Display Manager), where I log in and, by default, start KDE Plasma. 


Note the following lines from the systemctl status sddm command:

```
WARNING: CPU random generator seem to be failing, disable hardware random number generation
WARNING: RDRND generated: 0xffffffff 0xffffffff 0xffffffff 0xffffffff
```

It's a motherboard's firmware bug!



> The RDRAND function in Ryzen 3000 should be a high quality pseudo-random number generator but returns 0xFFFFFFFFFF every time.


Modern x86_64 CPUs - starting with Intel's Broadwell and AMD's Zen architectures - should have high-quality built-in random number generators (RNGs) that use thermal "noise" to quickly deliver high entropy pseudorandom numbers to anyone with access at the kernel level. 

RDRAND is, in turn, the instruction that provides these random numbers. 

Many applications use RDRAND, like WireGuard, for example. In my case, the log that reported the bug came from SystemD and SDDM (login manager). 

Identified in 2019 (and on several motherboards, the firmware has been updated/fixed, except on Asrock). 

Systemd works around this bug! Unless you are using the latest version of it. That is, a very current distro (with recent systemd), on current hardware that has not updated its firmware in the manufacturing process will give this error. Even though it was discovered in July 2019 with the release of this hardware, I purchased mine in April (2020), and its firmware was not updated. 

And finally: Updated motherboard BIOS. The error is gone!


Fonts:
https://arstechnica.com/gadgets/2019/10/how-a-months-old-amd-microcode-bug-destroyed-my-weekend/

https://linuxreviews.org/AMD_Ryzen_3000_series_CPUs_can%27t_do_Random_on_boot_causing_Boot_Failure_on_newer_Linux_distributions

https://www.forbes.com/sites/jasonevangelho/2019/07/12/amd-motherboard-patch-ryzen-3000-customers-affected-by-destiny-2-and-linux-boot-problems