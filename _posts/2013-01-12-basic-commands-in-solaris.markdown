---
layout: post
title:  "Basic Commands in Solaris"
date:   2013-01-12 23:03:52
categories: General
---

Recently, I have played around Solaris 11. Here are some basic commands in Solaris:

<strong>Package Management</strong>
<br>
Solaris is using PKG to manage packages, which is equivalent to YUM and RPM in Linux.
{% highlight bash %}
# Install MySQL package
pkgadd -d mysql-5.1.68-solaris10-x86_64.pkg

# Remove a package
pkgrm packagename

# List installed packages
pkginfo

# Install package from IPS. (Take the solaris-desktop package as an example)
pkg install solaris-desktop

# Uninstall IPS package.
pkg uninstall solaris-desktop
{% endhighlight %}

<strong>Mount and Umount</strong>
<br>
{% highlight bash %}
# Create a device from ISO file.
lofiadm -a /path/to/abc.iso

# Output is: /dev/lofi/1
/dev/lofi/1

# Mount the device
mount -o ro -F hsfs /dev/lofi/1 /mnt

# Umount the directory
umount /mnt

# Delete the device
lofiadm -d /dev/lofi/1
{% endhighlight %}

<strong>Service</strong>
<br>
It is equivalent to SERVICE command in Linux.
{% highlight bash %}
svcadm enable servicename
{% endhighlight %}

<strong>File System Mount File</strong>
<br>
Modify the file system mount information. It is equivalent to the /etc/fstab in Linux
{% highlight bash %}
/etc/vfstab
{% endhighlight %}

<strong>PS</strong>
<br>
PS command truncates the output in Solaris, so the command line options are limited to certain length. In order to display the full command line options, using the following:
{% highlight bash %}
ps auxww
{% endhighlight %}

<strong>NETSTAT</strong>
<br>
NETSTAT has no -l option in Solaris. Use GREP LISTEN to manipulate the output. Also the output of the NETSTAT doesn't contain the ":" between the HOST and PORT, which is the delimiter in Linux.
