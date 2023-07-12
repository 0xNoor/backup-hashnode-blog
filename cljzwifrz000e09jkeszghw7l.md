---
title: "Midterm Evalutions"
datePublished: Wed Jul 12 2023 15:53:54 GMT+0000 (Coordinated Universal Time)
cuid: cljzwifrz000e09jkeszghw7l
slug: midterm-evalutions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689177571025/d375f2eb-619a-42db-99ac-6bf4ee854eeb.jpeg
tags: gsoc, rtems, gsoc2023

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689167668998/8551fe96-3a75-48eb-889a-b7e3736662af.gif align="center")

Hello everynyan! Welcome back to my blogging page. Today I will be discussing my journey so far with my project. So without further adieu, let's get started

## Summary of what I have done so far:

* Importing genet drivers from the Freebsd tree.
    
* Implementing FDT support for the Raspberry Pi BSP.
    

But as of now non of them works as of now, I'm pretty tight on schedule this time because of my final semester exams but will be able to continue after I'm done with the exams (Ps: I wrote this blog too while I was having my exams [😉](https://prod.emojipedia.org/winking-face/) )

## More details about the topics which I've mentioned

### **Importing genet drivers from the Freebsd tree.**

This was the easy part, or that's what I thought.

![](https://en.meming.world/images/en/1/14/Perhaps.jpg align="center")

The first problem I faced was the FreeBSD revision version which was being used in RTEMS-libbbsd. The version which RTEMS-Libbsd was *commid-id#* `5d85e12f44ccb0e5728344a002c108ddc105e038` (Sep 24, 2019 last commit date).

And Genet drivers were introduced in the *commit-id#* `2cd0c529781a13caf32a9688dbc244dff529d6cf` (Apr 22, 2020 last commit date).

Hmmmmmm. This is a problem since the last update is almost 6 months behind. So possible solution? Maybe to maintain it out of the tree in the rtemsbsd folder. So Here we go importing the freebsd-org/sys/arm64/broadcom/genet/\* to rtemsbsd/sys/arm64/broadcom/genet/\*

Now Compilation time, add the relevant files to the libbsd.py, adding new module names as \`if\_genet\` and adding the BSP which supports the if\_genet module. anddddddddd boom. A lot of compilation errors. What to do now?

![](https://media.tenor.com/NpKEX5SFLUIAAAAi/cat-explosion.gif align="center")

So instead of using the latest patches of Genet drivers, let's try with the first patch which was introduced on April 22, 2020. A bit of fiddle diddle here, a bit over there and see if it changed anything. SURE IT DID. I was able to compile the code now, with a just few tweaking.

Let's see if the Genet drivers are being recognized or not. And they are not being recognized. Reason behind? genet drivers rely on simple-bus and simple-bus relies a bit on FDT support. So let's move ahead with the next completion (or failure)

### **Implementing FDT support for the Raspberry Pi BSP.**

My goal here is to just create a new minimal DTS for the raspberry pi 4B bsp which has only entry for the simple-bus, interrupt controller and genet ethernet controller. The DTS is inspired by the FreeBSD DTS and just a tiny bit from the Linux kernel. I haven't fully tested out the FDT yet, I was still working on that and then my final arrived.

*<mark>Q: Why not use the DTB which the raspberry pi repo provides?</mark>*

*Ans:* Technically we could, but it will require access to the raspberry pi videocore mailbox which we dont have support for yet, and we cant ship the DTB's too because of GPL license issue. hence we'll have to rely on the custom DTS which can be used. It will just be a minimal one.

## My updates from all the meetings are compiled here, I'll update them to the RTEMS-GSoC wiki soon.

28/06

`I'm working to implement FDT support for rpi bsp. It will be finished soon as I'm working a bit slow as of now, I have finals ahead of me. My progress would be almost slow or none for some upcoming week. Gedare I'll mail you as soon as my possible regarding the extension. Thanks`

21/06

`Hello, I'm working on how to implement FDT support for RPI4B which is might be for necessary for genet(ethernet) as ofwbus depends upon it. There are 2 path ahead of me of this, either use the dtb from raspberry pi firmware or either write a simple DTS for just ethernet and few IRQ. I'll decide which one to use and if possible try to look for another way.`

14/06

`Hello, I've imported the drivers into rtemsbsd as of now, made changes to some headers to at least make sure the file is being compiled, and it does. But the drivers arent being initialized. This might be because of inter-linking of files. I'll be looking into this issue in this week. I'm discussing this with Alan through private mails and public list. Kinsey helped a ton with this.`

7/06

`I'm trying to understand as of now how to import genet drivers from freebsd-13 into freebsd-12. There is a lot of dicussion in mailing list about this subject. This will require to backport the drivers. If this takes more than 2 weeks without making any progress, then i'll try to do the second half of my project instead which is to bring SMP support.`

31/05

`hello, I did not do much this week. I tried importing some codes from the freebsd-org tree, There are some issues which i'll likely to solve this weekend. Some of that include branches issue, importing code and more. I'm experimenting with the code as of now. Working with RPI is sometimes pain.`

24/05

`Hello, I made my libbsd repo ready for making changes, tested the bsp with some basic LIBBSD examples such as libcrypto and such. They worked fine. I will be now starting with my work as soon as possible. The first work would be to import genet drivers as it is from the freebsd-org to freebsd.`

17/05

`Hello everyone, I'm Mohd Noor Aman, A second time gsoc participant, both times with RTEMS. Last year, my project was to bring support for AArch64 raspberry pi 4b. This year, My project further aims to extend the functionality for the BSP, by adding Ethernet and SMP support for the raspberry pi 4b BSP. This is will include taking a reference from RT-thread, a similiar RTOS for the SMP part and importing genet drivers from the freebsd Tree. I have added myself to the contributor table and wrote a blog for the community bond period.`

## My patches which I was doing work on:

LIBBSD:

[https://github.com/RTEMS/rtems-libbsd/compare/master...0xNoor:rtems-libbsd:master](https://github.com/RTEMS/rtems-libbsd/compare/master...0xNoor:rtems-libbsd:master)

RTEMS:

[https://github.com/0xNoor/rtems/compare/master...0xNoor:rtems:noor-master](https://github.com/0xNoor/rtems/compare/master...0xNoor:rtems:noor-master)