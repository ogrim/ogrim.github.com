---
title: Suspend problem in Ubuntu 10.10 on ASUS N73JF (and others)
author: ogrim
layout: post
permalink: /2010/12/suspend-problem-in-ubuntu-10-10-on-asus-n73jf-and-others/
comments: true
---
**Update 26th August 2011:** *Solution tested and working with Debian Squeeze (my new main distro). A commenter reported it works on Mint, so I guess it works on most Debian-based distros. Another commenter had success on a Gigabyte motherboard, which is very interesting indeed.*

After 3.5 years I finally caved in and got a new laptop. Screen resolution, lagging editors and general sloppyness where the major factors for ditching the old one. I have been quite pleased with the quality on my ASUS eeePC 1015PE, so I decided to go for another ASUS computer. As I am to use this machine for development, I&#8217;d like to have a large screen; the old one left me frustrated at the lack of screen estate and the crappy resolution. I therefore got me a 17,3&#8243; ASUS N73JF, with enough power to last me just as long as the old one, I hope!

Since the new laptop has a somewhat decent graphics card, I decided to let the Windows install it came with stay, to be used for games and the likes. I of course installed Ubuntu 10.10 64-bit right away after booting the laptop to Windows 7 to check it out. **It took longer booting to the preinstalled Windows 7, than it took installing Ubuntu from scratch!** In addition to this, the Windows install was defiled with crapware. No wonder people buy Macs. Enough rambling; over to the problem at hand.

I did encounter a problem with getting the Suspend working in Ubuntu. After checking the fantastic ubuntuforums.org, I found people with similar issues. [This thread][1] holds the answers, but it seems the fix is more technical than it should be. However it will probably get included in some patch in the future.

At least for the N73JF, you need to create two files.

``` bash
sudo touch /etc/pm/sleep.d/20_custom-ehci_hcd
sudo touch /etc/pm/sleep.d/20_custom-xhci_hcd
```

To open a GUI editor like gedit, with root privileges, you can use the following command:

``` bash
gksudo gedit
```

Keep in mind this is dangerous if you edit the wrong files, so only open the ones we created with the touch-command. If you want to open the file directly, you can append the path to the filename onto the gedit command like so:

``` bash
gksudo gedit /etc/pm/sleep.d/20_custom-ehci_hcd
```

When you have opened the files in your favorite editor, we need to enter some scripts. In `20_custom-ehci-hcd` put in:

``` bash
#!/bin/sh
# File: "/etc/pm/sleep.d/20_custom-ehci_hcd".
TMPLIST=/tmp/ehci-dev-list

case "${1}" in
        hibernate|suspend)
    echo -n '' &gt; $TMPLIST
          for i in `ls /sys/bus/pci/drivers/ehci_hcd/ | egrep '[0-9a-z]+\:[0-9a-z]+\:.*$'`; do
              # Unbind ehci_hcd for first device XXXX:XX:XX.X:
               echo -n "$i" | tee /sys/bus/pci/drivers/ehci_hcd/unbind
           echo "$i" &gt;&gt; $TMPLIST
          done
        ;;
        resume|thaw)
    for i in `cat $TMPLIST`; do
              # Bind ehci_hcd for first device XXXX:XX:XX.X:
              echo -n "$i" | tee /sys/bus/pci/drivers/ehci_hcd/bind
    done
    rm $TMPLIST
        ;;
esac
```

In `20\_custom-xhci\_hcd` put in:

``` bash
#!/bin/sh
# File: "/etc/pm/sleep.d/20_custom-xhci_hcd".
TMPLIST=/tmp/xhci-dev-list

case "${1}" in
        hibernate|suspend)
    echo -n '' &gt; $TMPLIST
          for i in `ls /sys/bus/pci/drivers/xhci_hcd/ | egrep '[0-9a-z]+\:[0-9a-z]+\:.*$'`; do
              # Unbind ehci_hcd for first device XXXX:XX:XX.X:
               echo -n "$i" | tee /sys/bus/pci/drivers/xhci_hcd/unbind
           echo "$i" &gt;&gt; $TMPLIST
          done
        ;;
        resume|thaw)
    for i in `cat $TMPLIST`; do
              # Bind ehci_hcd for first device XXXX:XX:XX.X:
              echo -n "$i" | tee /sys/bus/pci/drivers/xhci_hcd/bind
    done
    rm $TMPLIST
        ;;
esac
```

Now you must make these files executable with these two commands:

``` bash
chmod +x /etc/pm/sleep.d/20_custom-ehci_hcd
chmod +x /etc/pm/sleep.d/20_custom-xhci_hcd
```

A user on the forum reported he didn&#8217;t need the extra file as proposed in the first solution. I did not test without this file, as it wasn&#8217;t working until I added the xhci-related file. I suggest you try the steps outlined here first, and attempt these last steps only if it doesn&#8217;t work.

Make this file:

```  bash
sudo touch /etc/pm/config.d/usb3-suspend-workaround
```

Open the file, and put this in:

``` bash
#File: "/etc/pm/config.d/usb3-suspend-workaround".
SUSPEND_MODULES="xhci"
```

He didn&#8217;t say if it needs to be executable, I&#8217;m assuming it should and that it couldn&#8217;t hurt:

``` bash
chmod +x /etc/pm/config.d/usb3-suspend-workaround
```

I hope you get it working. You should also check out [the source for this post][1], as pointed out earlier. The forum might contain new information, after this is published.

 [1]: http://ubuntuforums.org/showthread.php?t=1444822
