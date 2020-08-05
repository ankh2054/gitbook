---
description: >-
  Automated security updates is a great way to ensure your system automatically
  installs all security updates.
---

# Ubuntu automated security updates

### **Step 1: package** installation

Install the `unattended-upgrades` package:

```
sudo apt install unattended-upgrades
```

### **Step 2: configure automatic updates**

Edit the configuration file with your favourite text editor:

```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```

There will be alot of other information, but the lines you want to uncomment are:

```bash
"${distro_id}:${distro_codename}";
"${distro_id}:${distro_codename}-security";
```

That tells Ubuntu to automatically perform security updates. Your file will look similar to this after making the changes,

```bash
Unattended-Upgrade::Allowed-Origins {
  "${distro_id}:${distro_codename}";
	"${distro_id}:${distro_codename}-security";
	// Extended Security Maintenance; doesn't necessarily exist for
	// every release and this system may not have it installed, but if
	// available, the policy for updates is such that unattended-upgrades
	// should also install from here by default.
	"${distro_id}ESM:${distro_codename}";
//	"${distro_id}:${distro_codename}-updates";
//	"${distro_id}:${distro_codename}-proposed";
//	"${distro_id}:${distro_codename}-backports";
};
```



### **Step3 - Enabling Unattended Automatic Updates** 

To enable automatic updated, you need to ensure that the apt configuration file `/etc/apt/apt.conf.d/20auto-upgrades` contains at least the following two lines. It's usually included by default.

```text
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

_The above configuration updates the package list, and installs available updates every day._

For bonus points and a cleaner machine, add this to to anove file to clean your download cache every 7 days. 

```bash
APT::Periodic::AutocleanInterval "7";
```



### **Bonus step - You can test whether it works by doing a dry run.**

```bash
sudo unattended-upgrades --dry-run --debug
```

**Your output will look similar to this.**

```bash
pkgs that look like they should be upgraded: 
Fetched 0 B in 0s (0 B/s)                                                                                                                                                                            
fetch.run() result: 0
blacklist: []
whitelist: []
Option --dry-run given, *not* performing real actions
Packages that will be upgraded: 
InstCount=0 DelCount=0 BrokenCount=0
```

