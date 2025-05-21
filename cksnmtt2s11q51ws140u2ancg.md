---
title: "Etckeeper"
datePublished: Sun Aug 22 2021 19:57:31 GMT+0000 (Coordinated Universal Time)
cuid: cksnmtt2s11q51ws140u2ancg
slug: etckeeper-versioning-etc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1629661615172/ka1UxINEf.png
tags: linux, github, git, gitlab, versioning

---

One of the many advantages of Linux, UNIX and similar operating systems is that everything is a file and that most of their configuration is done through text files, allowing you to easily read and write to them with any tool you choose.

There are several tools to monitor, automate and control your system settings and with them, you can:

- Check recent changes in the environment, 
- restore if something is deleted, 
- rollback with greater security if a change generates errors, 
- easily share changes with a team, 
- security level ensuring the integrity of settings when your environment is large enough not to know anything more 'by heart' and end up with millions of copies of configuration files (those who never logged into a server, and inside some /etc directory found thousands of versions with suffixes or prefixes bkp, v1, v2, old, and others...).

So, have you ever thought about using git (or other) to review or revert changes made in /etc or even "push" the repository to different places?

**etckeeper** is a collection of tools that allow */etc* to be stored in a git, mercurial, bazaar or darcs repository. 

By default, the etckeeper uses git.

Besides being simple to use, etckeeper is modular and highly configurable, and if you already know git or svn it will be even easier 😉

Among the additional features it has, etckeeper connects to package managers like apt to automatically commit changes made during package updates. 
Tracks file metadata that git does not normally support, but which is of utmost importance to /etc such as /etc/shadow permissions.

Other tools, such as configuration management and distribution package management, also track settings, but do not track all files in version control.

Configuration management systems (CMS) such as Puppet, Chef and Ansible do not keep track of all files in /etc. 
For many files, they check that certain contents are in place and ignore the rest of the file. For example, Puppet will check if a user exists in /etc/passwd, but will ignore changes to accounts it is not managing. 

When an update is made to a package with a service already configured and running, it will bring a new configuration file, informing that another one already exists, but it does not track the changes, having the sysadmin to compare the new one and take advantage of the new features , ignore, replace, anyway...

etckeeper will keep track of everything except the repo directory and whatever you choose to ignore in .gitignore, etckeeper will help with version control by tracking important metadata in the .etckeeper file.

etckeeper augments the underlying version control system by tracking this important metadata in /etc/.etckeeper. 
Once again, this file is tracked in the version control system. 
etckeeper also has hooks to work with the package management system and check in changes after package installations and updates.

The latest version is 1.18.16 

To install it, just check your distribution package manager (based on RedHat and Debian already have as CentOS and Ubuntu). If you prefer, the source is in git  git.joeyh.name/index.cgi/etckeeper.git (note: before 'make install', edit the conf for your Linux distribution).

Once installed, use init to initialize the repository and then check the current state:


```
sudo etckeeper init
sudo etckeeper commit -m "Initial commit"
``` 


Once started, /etc will be a repository on your version control system, just like any other repo. 
You can enter VCS commands directly (like git status, git commit, etc...) or use etckeeper:

```
sudo etckeeper vcs git status
sudo etckeeper vcs git commit
```

If you use some tool like Gitkraken or SmartGit, you will be able to open it normally.

Viewing with Gitkraken (on the cover photo), I enabled etckeeper on my laptop and added as 'remote' origin. I put a private repo on my GitHub account (github.com/Esl1h).

Website: etckeeper.branchable.com

%[https://youtu.be/riyXmtKO0L4]
