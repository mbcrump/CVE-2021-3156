## CVE-2021-3156

## Note: These instructions are my own and notes from a stream that I did. If anything is wrong, then let me know. Trust official sources first!

Hello everyone, 

A couple of days back, a serious heap-based buffer overflow has been discovered in sudo that is exploitable by any local user. It has been given the name Baron Samedit by its discoverer. The bug can be leveraged to elevate privileges to root, even if the user is not listed in the sudoers file. User authentication is not required to exploit the bug. It is commonly referred as [CVE-2021-3156](https://www.qualys.com/2021/01/26/cve-2021-3156/baron-samedit-heap-based-overflow-sudo.txt).

### See it in action

You can see it in action [here](https://vimeo.com/504872555) that several security researchers have recorded. 

### Sudo versions affected: 

Sudo versions 1.8.2 through 1.8.31p2 and 1.9.0 through 1.9.5p1 are affected.

**Keep in mind:** Unless the patches have been backported, as is the case for Ubuntu 18.04, in which case it [may not need to be 1.9.5p2](https://launchpad.net/ubuntu/+source/sudo/1.8.21p2-3ubuntu1.4)

**From TroyWolf** 

For Ubuntu users, the patched sudo version appears to be related to the version of the OS you are running. The Ubuntu site shows this release-specific information for the flaw:

* Ubuntu 21.04 (Hirsute Hippo) Released (1.9.4p2-2ubuntu2)
* Ubuntu 20.10 (Groovy Gorilla) Released (1.9.1-1ubuntu1.1)
* Ubuntu 20.04 LTS (Focal Fossa) Released (1.8.31-1ubuntu1.2)
* Ubuntu 18.04 LTS (Bionic Beaver) Released (1.8.21p2-3ubuntu1.4)
* Ubuntu 16.04 LTS (Xenial Xerus) Released (1.8.16-0ubuntu1.10)
* Ubuntu 14.04 ESM (Trusty Tahr) Released (1.8.9p5-1ubuntu1.5+esm6)

### Are you vulnerable? 

**USE WITH CAUTION** : Open your terminal and use `sudo sudo -V` to check your version number. See notes above for what versions are affected and read the note.

### A More reliable way to check

Run `sudoedit -s /` to detect if you are vulnerable. 

You can also try:

```
sudoedit -s '\' `perl -e 'print "A" x 65536'` 
```

**bonyt from hackernews** pointed out that macOS doesn't have sudoedit, but you can make a symlink to it, as sudo is effectively a multi-call binary:

```
ln -s `which sudo` sudoedit
```

and then call it:

```
./sudoedit
```

Based on the response, It’s possible to understand if the host is vulnerable or not:

Vulnerable if responds an error starting with sudoedit: **`sudoedit: /: not a regular file`** or `segfault`
Not Vulnerable or patched if responds an error starting with `usage:`

### Got containers or K8s?

[Falco](https://falco.org/) is the CNCF open-source project for runtime threat detection for containers and Kubernetes.

You can use a [rule](https://github.com/falcosecurity/falco/issues/1540) to detect and prevent if a user attempts it.

**pawan328 from hackernews** pointed out that you can view this [blog post](https://sysdig.com/blog/cve-2021-3156-sudo-falco/) for additional details.

### Fix it - the bug has been fixed in sudo 1.9.5p2

#### For Linux distros (that might be running on a Pi or a another computer) including WSL

```bash
sudo apt-get update
sudo apt-get --only-upgrade install sudo
```

### Watch the Video

I have a video discussing this and showing all the methods shown above. 

[![](http://img.youtube.com/vi/iRIXXUNkgAM/0.jpg)](http://www.youtube.com/watch?v=iRIXXUNkgAM "")

Which covers: 

00:00​ - Introduction

01:29​ - What's a CVE?

02:22​ - What's the bug?

05:30​ - Exploit shown in action

13:07​ - Sudo versions affected

13:48​ - WSL Windows Subsystem for Linux

15:11​ - Detecting if you are vulnerable through sudo (WSL)

19:21​ - Detecting if you are vulnerable through sudo (Ubuntu)

20:53​ - Detecting if you are vulnerable through commands

24:58​ - What to do if you are running K8s or containers

27:54​ - Examine Sudo versions

28:30​ - Patching WSL 

30:17​ - Patching Ubuntu 

32:09​ - Where can I get exploit.c? 

33:59​ - Patching Ubuntu (part 2)

38:10​ - Patching WSL (part 2)

46:35​ - GitHub search results

47:28​ - Patching WSL (part 3)

### Conclusion

I [stream](https://twitch.tv/mbcrump) on Twitch on Wednesday's at Noon PST and on Sat/Sun at 10 AM PST or you can watch the condensed version by subscribing to my [YouTube](https://youtube.com/mbcrump). 

Stay connected with me on social platforms for security and software development news.

-[Twitter](https://twitter.com/intent/follow?screen_name=mbcrump) | [Twitch](https://twitch.tv/mbcrump) | [Instagram](https://instagram.com/mbcrump) | [YouTube](https://youtube.com/mbcrump) | [GitHub](https://github.com/mbcrump) | [Website](https://www.michaelcrump.net)
