树莓派搭建Perl开发环境
=============

> 树莓派是一个开放的平台，只有想不到，没有做不到。



 ![Getting started with Perl on the Raspberry Pi](https://opensource.com/sites/default/files/styles/image-full-size/public/images/life/raspberry_pi_blue_board.jpg?itok=01NR5MX4 "Getting started with Perl on the Raspberry Pi")
>Image by :opensource.com

    前几天我在SVPerl上说用树莓派搭建了个Perl环境的话题，就有人冒出来问我，咦？那玩意儿不是只能用来折腾Python么？我对他说，恭喜你，答对了，余也是这么想地（我也是这么想的）。但其实树莓派是一个开放的平台，什么都可以做。树莓派的官方系统可以跑所有的编程语言，Perl、Python还有其它常见的语言更是原生支持。
    这其中原因取决于树莓派的创始团队的理念，不做任何限制。树莓派的创始人 Eben Upton 教授在解释“树莓派”的命名时说，这个产品命名为“派”， 一个原因是 Pi 听起来很像 Python，他很喜欢这门语言，他推荐把这门语言作为小孩子学习编程的入门语言，但是这并不影响他和他和团队创造了一款通用型计算机而不只是一个开发板。树莓派支持的众多软件也都是开源的，对我们没有任何限制，只要你想到的，应有尽有。

更多关于树莓派

*   [树莓派最新情报][1]
*   [什么是树莓派？][2]
*   [树莓派入门指南][3]
*   [共享你的树莓派hack杰作和制作教程][4]

    接下来我要介绍的是我用 Perl 写的镜像刻录软件，我给他取名 PiFlash ，用户不懂Perl也没关系，不需要看脚本，只要能把脚本跑起来就能成功。
这个脚本的特点之一是防止粗手指误输入“sudo rm -rf /”这样的命令 ，而且操作简单，一键完成。同类产品有windows平台的Win32DiskImager，傻瓜式的，点几下就把树莓派系统刻录到SD卡里了，这样的软件在 Mac 系统上也有，但是 Linux 系统上一直没有，不过现在有了，没错，就是这个。当然，这个软件对高级用户来说更是得心应手，比如我。哈哈，这也是我写他的原因。

Open source software has a long tradition of new projects starting because an author wanted to "scratch their own itch," or to solve their own problems. That's the way Eric S. Raymond described it in his 1997 paper and 1999 book?"[大教堂与市集The Cathedral and the Bazaar][8]," which defined the open source software development methodology. I wrote PiFlash to fill a need for Linux users like myself.
开源软件一直有一个陋习，就是在项目一开始的时候，作者只满足他自己的需求，这也是 Eric S. Raymond 在[大教堂与市集][8]这本书中所批判的，这本书也是 Eric 97年发表的的论文，定义了开源软件的方法论。因此我在写的这个软件的时候，就遵照了做有效开源的原则，一方面满足自己的需求，同时也考虑了其它人的需要。

### 下载官方镜像Downloadable system images

When setting up a Raspberry Pi, you first need to download an operating system for it. We call it a "system image" file. Once you download it to your desktop, laptop, or even another Raspberry Pi, you have to write or "flash" it to an SD card. The details are covered online already. It can be a bit tricky to do manually because getting the system image on the whole SD card and?not on a partition matters. The system image will actually contain at least one partition of its own because the Raspberry Pi's boot procedure needs a FAT32 filesystem partition from which to start. Other partitions after the boot partition can be any filesystem type supported by the OS kernel.

In most cases on the Raspberry Pi, we're running some distribution with a Linux kernel. Here's a list of common system images that you can download for the Raspberry Pi (but there's nothing to stop you from building your own from scratch).

The?["NOOBS"][9]?system from the Raspberry Pi Foundation is their recommended system for new users. It stands for "New Out of the Box System." It's obviously intended to sound like the term "noob," short for "newbie." NOOBS starts a Raspbian-based Linux system, which presents a menu that you can use to automatically download and install several other system images on your Raspberry Pi.

[Raspbian?][10][Linux][11]?is Debian Linux specialized for the Raspberry Pi. It's the official Linux distribution for the Raspberry Pi and is maintained by the Raspberry Pi Foundation. Nearly all Raspberry Pi software and drivers start with Raspbian before going to other Linux distributions. It runs on all models of the Raspberry Pi. The default installation includes Perl.

Ubuntu Linux (and the community edition Ubuntu MATE) includes the Raspberry Pi as one of its supported platforms for the ARM (Advanced RISC Machines) processor. [RISC (Reduced Instruction Set Computer) architecture] Ubuntu is a commercially supported open source variant of Debian Linux, so its software comes as DEB packages. Perl is included. It only works on the Raspberry Pi 2 and 3 models with their 32-bit ARM7 and 64-bit ARM8 processors. The ARM6 processor of the Raspberry Pi 1 and Zero was never supported by Ubuntu's build process.

[Fedora Linux][12]?supports the Raspberry Pi 2 and 3 as of Fedora 25\. Fedora is the open source project affiliated with Red Hat. Fedora serves as the base that the commercial RHEL (Red Hat Enterprise Linux) adds commercial packages and support to, so its software comes as RPM (Red Hat Package Manager) packages like all Red Hat-compatible Linux distributions. Like the others, it includes Perl.

[RISC OS][13]?is a single-user operating system made specifically for the ARM processor. If you want to experiment with a small desktop that is more compact than Linux (due to fewer features), it's an option. Perl runs on RISC OS.

[RaspBSD][14]?is the Raspberry Pi distribution of FreeBSD. It's a Unix-based system, but isn't Linux. As an open source Unix, form follows function and it has many similarities to Linux, including that the operating system environment is made from a similar set of open source packages, including Perl.

[OSMC][15], the Open Source Media Center, and?[LibreElec][16]?are TV entertainment center systems. They are both based on the Kodi entertainment center, which runs on a Linux kernel. It's a really compact and specialized Linux system, so don't expect to find Perl on it.

[Microsoft?][17][Windows IoT Core][18]?is a new entrant that runs only on the Raspberry Pi 3\. You need Microsoft developer access to download it, so as a Linux geek, that deterred me from looking at it. My PiFlash script doesn't support it, but if that's what you're looking for, it's there.

### The PiFlash script

If you look at the Raspberry Pi 's?[SD card flashing][19][?instructions][20], you'll see the instructions to do that from Windows or Mac involve downloading a tool to write to the SD card. But for Linux systems, it's a set of instructions to do manually. I've done that manual procedure so many times that it triggered my software-developer instinct to automate the process, and that's where the PiFlash script came from. It's tricky because there are many ways a Linux system can be set up, but they are all based on the Linux kernel.

I always imagined one of the biggest potential errors of the manual procedure is accidentally erasing the wrong device, instead of the SD card, and destroying the data on a hard drive that I?wanted to keep. In my presentation at SVPerl, I was surprised to find someone in the audience who has made that mistake (and wasn't afraid to admit it). Therefore, one of the purposes of the PiFlash script, to provide safety for new users by refusing to erase a device that isn't an SD card, is even more needed than I expected. PiFlash will also refuse to overwrite a device that contains a mounted filesystem.

For experienced users, including me, the PiFlash script offers the convenience of automation. After downloading the system image, I don't have to uncompress it or extract the system image from a zip archive. PiFlash will extract it from whichever format it's in and directly flash the SD card.

I posted?[PiFlash and its instructions][21]?on GitHub.

It's a command-line tool with the following usages:

**piflash [--verbose] input-file output-device**

**piflash [--verbose] --SDsearch**

The?**input-file**?parameter is the system image file, whatever you downloaded from the Raspberry Pi software distribution sites. The?**output-device**?parameter is the path of the block device for the SD card you want to write to.

Alternatively, use?**--SDsearch**?to print a list of the device names of SD cards on the system.

The optional?**--verbose**?parameter is useful for printing out all of the program's state data in case you need to ask for help, submit a bug report, or troubleshoot a problem yourself. That's what I used for developing it.

This example of using the script writes a Raspbian image, still in its zip archive, to the SD card at?**/dev/mmcblk0**:

**piflash 2016-11-25-raspbian-jessie.img.zip /dev/mmcblk0**

If you had specified?**/dev/mmcblk0p1**?(the first partition on the SD card), it would have recognized that a partition is not the correct location and refused to write to it.

One tricky aspect is recognizing which devices are SD cards on various Linux systems. The example with?**mmcblk0**?is from the PCI-based SD card interface on my laptop. If I used a USB SD card interface, it would be?**/dev/sdb**, which is harder to distinguish from hard drives present on many systems. However, there are only a few Linux block drivers that support SD cards. PiFlash checks the parameters of the block devices in both those cases. If all else fails, it will accept USB drives which are writable, removable and have the right physical sector count for an SD card.

I think that covers most cases. However, what if you have another SD card interface I haven't seen? I'd like to hear from you. Please include the?**--verbose****?--SDsearch**?output, so I can see what environment was present on your system when it tried. Ideally, if the PiFlash script becomes widely used, we should build up an open source community around maintaining it for as many Raspberry Pi users as we can.

### CPAN modules for Raspberry Pi

CPAN is the?[Comprehensive Perl Archive Network][22], a worldwide network of download mirrors containing a wealth of Perl modules. All of them are open source. The vast quantity of modules on CPAN has been a huge strength of Perl over the years. For many thousands of tasks, there is no need to re-invent the wheel, you can just use the code someone else already posted, then submit your own once you have something new.

As Raspberry Pi is?a full-fledged Linux system,?most CPAN modules will run normally on it, but I'll focus on some that are specifically for the Raspberry Pi's hardware. These would usually be for embedded systems projects like measurement, control, or robotics. You can connect your Raspberry Pi to external electronics via its GPIO (General-Purpose Input/Output) pins.

Modules specifically for accessing the Raspberry Pi's GPIO pins include?[Device::SMBus][23],?[Device::I2C][24],?[Rpi::PIGPIO][25],?[Rpi::SPI][26],?[Rpi::WiringPi][27],?[Device::WebIO::RaspberryPi][28]?and?[Device::PiGlow][29]. Modules for other embedded systems with Raspberry Pi support include?[UAV::Pilot::Wumpus::Server::Backend::RaspberryPiI2C][30],?[RPi::DHT11][31]?(temperature/humidity),?[RPi::HCSR04][32]?(ultrasonic),?[App::RPi::EnvUI][33]?(lights for growing plants),??[RPi::DigiPot::MCP4XXXX][34]?(potentiometer),?[RPi::ADC::ADS][35]?(A/D conversion),?[Device::PaPiRus][36]?and?[Device::BCM2835::Timer][37]?(the on-board timer chip).

### Examples

Here are some examples of what you can do with Perl on a Raspberry Pi.

### Example 1: Flash OSMC with PiFlash and play a video

For this example, you'll practice setting up and running a Raspberry Pi using the OSMC (Open Source Media Center).

*   Go to?[RaspberryPi.Org][5]. In the downloads area, get the latest version of OSMC.
*   Insert a blank SD card in your Linux desktop or laptop. The Raspberry Pi 1 uses a full-size SD card. Everything else uses a microSD, which may require a common adapter to insert it.
*   Check "cat /proc/partitions" before and after inserting the SD card to see which device name it was assigned by the system. It could be something like?**/dev/mmcblk0**?or?**/dev/sdb**. Substitute your correct system image file and output device in a command that looks like this:

**? ? ? ? ? ?piflash OSMC_TGT_rbp2_20170210.img.gz /dev/mmcblk0**

*   Eject the SD card. Put it in the Raspberry Pi and boot it connected to an HDMI monitor.
*   While OSMC is setting up, get a USB stick and put some videos on it. For purposes of the demonstration, I suggest using the "youtube-dl" program to download two videos. Run "youtube-dl OHF2xDrq8dY" (The Bloomberg "Hello World" episode about UK tech including Raspberry Pi) and "youtube-dl nAvZMgXbE9c" (CNet's Top 5 Raspberry Pi projects). Move them to the USB stick, then unmount and remove it.
*   Insert the USB stick in the OSMC Raspberry Pi. Follow the Videos menu to the external device.
*   When you can play the videos on the Raspberry Pi, you have completed the exercise. Have fun.

### Example 2: A script to play random videos from a directory

This example uses a script to shuffle-play videos from a directory on the Raspberry Pi. Depending on the videos and where it's installed, this could be a kiosk display. I wrote it to display videos while using indoor exercise equipment.

*   Set up a Raspberry Pi to boot Raspbian Linux. Connect it to an HDMI monitor.
*   Download my?["do-video" script][6]?from GitHub and put it on the Raspberry Pi.
*   Follow the installation instructions on the page. The main thing is to install the?**omxplayer**?package, which plays videos smoothly using the Raspberry Pi's hardware video acceleration.
*   Put some videos in a directory called Videos under the home directory.
*   Run "do-video" and videos should start playing.

### Example 3: A script to read GPS data

This example is more advanced and optional, but it shows how Perl can read from external devices. At my "Perl on Pi" page on GitHub from the previous example, there is also a?**gps-read.pl**?script. It reads NMEA (National Marine Electronics Association) data from a GPS via the serial port. Instructions are on the page, including parts I used from AdaFruit Industries to build it, but any GPS that outputs NMEA data could be used.

With these tasks, I've made the case that you really can use Perl as well as any other language on a Raspberry Pi. I hope you enjoy it.

--------------------------------------------------------------------------------

作者简介：

Ian Kluft - Ian has had parallel interests since grade school in computing and flight. He was coding on Unix before there was Linux, and started on Linux 6 months after the kernel was posted. He has a masters degree in Computer Science and is a CSSLP (Certified Secure Software Lifecycle Professional). On the side he's a pilot and a certified flight instructor. As a licensed Ham Radio operator for over 25 years, experimentation with electronics has evolved in recent years to include the Raspberry Pi
Ian Kluft - Ian 从小学开始就对计算机和飞机感兴趣，还没有Linux那会儿，他就在Unix上开始编程了。在Linux内核发布6个月的时候，他加入了Linux阵营。他有一个计算机科学的硕士学位，是一个 CSSLP （软件生命周期安全认证专家）。他还是一个飞机驾驶员，并取得了飞行教练员资格证。
------------------

via: https://opensource.com/article/17/3/perl-raspberry-pi

作者：[Ian Kluft ][a]
译者：[译者ID](https://github.com/译者ID)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:https://opensource.com/users/ikluft
[1]:https://opensource.com/tags/raspberry-pi?src=raspberry_pi_resource_menu
[2]:https://opensource.com/resources/what-raspberry-pi?src=raspberry_pi_resource_menu
[3]:https://opensource.com/article/16/12/getting-started-raspberry-pi?src=raspberry_pi_resource_menu
[4]:https://opensource.com/article/17/2/raspberry-pi-submit-your-article?src=raspberry_pi_resource_menu
[5]:http://raspberrypi.org/
[6]:https://github.com/ikluft/ikluft-tools/tree/master/perl-on-pi
[7]:https://opensource.com/article/17/3/perl-raspberry-pi?rate=OsZH1-H_xMfLtSFqZw4SC-_nyV4yo_sgKKBJGjUsbfM
[8]:http://www.catb.org/~esr/writings/cathedral-bazaar/
[9]:https://www.raspberrypi.org/downloads/noobs/
[10]:https://www.raspberrypi.org/downloads/raspbian/
[11]:https://www.raspberrypi.org/downloads/raspbian/
[12]:https://fedoraproject.org/wiki/Raspberry_Pi#Downloading_the_Fedora_ARM_image
[13]:https://www.riscosopen.org/content/downloads/raspberry-pi
[14]:http://www.raspbsd.org/raspberrypi.html
[15]:https://osmc.tv/
[16]:https://libreelec.tv/
[17]:http://ms-iot.github.io/content/en-US/Downloads.htm
[18]:http://ms-iot.github.io/content/en-US/Downloads.htm
[19]:https://www.raspberrypi.org/documentation/installation/installing-images/README.md
[20]:https://www.raspberrypi.org/documentation/installation/installing-images/README.md
[21]:https://github.com/ikluft/ikluft-tools/tree/master/piflash
[22]:http://www.cpan.org/
[23]:https://metacpan.org/pod/Device::SMBus
[24]:https://metacpan.org/pod/Device::I2C
[25]:https://metacpan.org/pod/RPi::PIGPIO
[26]:https://metacpan.org/pod/RPi::SPI
[27]:https://metacpan.org/pod/RPi::WiringPi
[28]:https://metacpan.org/pod/Device::WebIO::RaspberryPi
[29]:https://metacpan.org/pod/Device::PiGlow
[30]:https://metacpan.org/pod/UAV::Pilot::Wumpus::Server::Backend::RaspberryPiI2C
[31]:https://metacpan.org/pod/RPi::DHT11
[32]:https://metacpan.org/pod/RPi::HCSR04
[33]:https://metacpan.org/pod/App::RPi::EnvUI
[34]:https://metacpan.org/pod/RPi::DigiPot::MCP4XXXX
[35]:https://metacpan.org/pod/RPi::ADC::ADS
[36]:https://metacpan.org/pod/Device::PaPiRus
[37]:https://metacpan.org/pod/Device::BCM2835::Timer
[38]:https://opensource.com/user/120171/feed
[39]:https://opensource.com/article/17/3/perl-raspberry-pi#comments
[40]:https://opensource.com/users/ikluft
