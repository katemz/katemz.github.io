---
layout: post
title:  "BeagleBone Black - first steps"
date:   2022-03-05 22:21:07 +0100
categories: beaglebone
---

This post is containing my notes from the first use of BeagleBone Black. My host is Ubuntu 20.04.

The board is coming to you ready-to-use. Just plug it into laptop via USB and wait until it mounts as external memory. In network settings you should see two new Ethernet connections. There is some instruction in `BEAGLEBONE:/START.md`, which is a nice starting point.

### Flashing SD card with Debian
0. Prepare microSD card.
1. Dowload newest Debian image from [https://beagleboard.org/latest-images](https://beagleboard.org/latest-images). I used *Buster IoT (without graphical desktop) for BeagleBone and PocketBeagle via microSD card*.
2. Download [Balena Etcher](https://www.balena.io/etcher/).
3. Flash SD using Balena Etcher.
4. After you put ready SD into BBB and restart it, BBB boots from the SD card.


### Accessing BBB via SSH

Login via ssh. On the first time do it via terminal to see the initial password (*temppwd*). Later on you can use PuTTy.

{% highlight bash %}
ssh debian@192.168.7.2
{% endhighlight %}


### First program (native compilation)

SSH to your Beaglebone and create first program.

{% highlight bash %}
vim program.cpp
{% endhighlight %}

First program may look like this:

{% highlight cpp %}
#include <iostream>

using namespace std;
int main(int argc, char** argv)
{  
  cout<<"Hello World!";
  return 0;
}
{% endhighlight %}

Save and compile. Check out the binary file with file command:
{% highlight bash %}
g++ -o program program.cpp
file program
hello: ELF 32-bit LSB pie executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, BuildID[sha1]=45427c64816b4f89a4ce05e9a65d81b1fa701538, not stripped
{% endhighlight %}

The program is successfully compiled for ARM architecture. You may run it on your Beaglebone Black platform.

# First program (cross compilation on Ubuntu)

To make cross compilation work on Ubuntu I mostly followed the post: *[Guide cross compiling for the beaglebone black on reddit](https://www.reddit.com/r/BeagleBone/comments/du4lwb/guide_cross_compiling_for_the_beaglebone_black_on/)*. In summary, on your host machine do the following:


{% highlight bash %}
sudo cp sources.list sources.list.old
sudo vim sources.list
{% endhighlight %}
Copy paste text from here: [https://gist.github.com/josephlr/5034c933bbcfddc25a9275037821b048](https://gist.github.com/josephlr/5034c933bbcfddc25a9275037821b048)
  
{% highlight bash %}
sudo dpkg --add-architecture armhf
dpkg --print-architecture
sudo apt-get update
sudo apt-get install dpkg-cross
sudo apt-get install g++-arm-linux-gnueabihf
sudo apt-get install gcc-arm-linux-gnueabihf
sudo apt-get install crossbuild-essential-armhf
sudo apt-get install build-essentia
{% endhighlight %}

After these steps I was able to cross compile programs for Beaglebone Black. Let's create the same program as earlier, but do this on your host. Compile it with following:

{% highlight bash %}
arm-linux-gnueabihf-g++ program.cpp -o program
{% endhighlight %}

If you now try to run the program on your host it will fail. Instead, copy this to BBB and run it there.
{% highlight bash %}
scp ./program debian@192.168.7.2:/home/debian
{% endhighlight %}

That's it!
