---
title: "GSoC 2022 Final Submission: RTEMS SMP Support for Raspberry Pi 4B"
datePublished: Sun Sep 24 2023 16:28:34 GMT+0000 (Coordinated Universal Time)
cuid: clmxoc24p000108igd7xs5wy4
slug: gsoc-2022-final-submission-rtems-smp-support-for-raspberry-pi-4b
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1695543764262/d4569386-32cf-416b-97fc-ccaac340633b.jpeg

---

![How to Make Your Raspberry Pi 4 Faster with a 64 Bit Kernel | by Al  Williams | For Linux Users | Medium](https://miro.medium.com/v2/resize:fit:0/1*7w2jtetqJU8wcvh32044Cw.png align="left")

### Hey Everyone,

I've completed my GSoC Project but......... Only partially **:(** But I'll give an explanation on why wasn't I able to. I'll be explaining my journey, the obstacles, the hurdles and many more things to count so let's get started.

### Repositories:

#### LIBBSD:

[https://github.com/RTEMS/rtems-libbsd/compare/master...0xNoor:rtems-libbsd:master](https://github.com/RTEMS/rtems-libbsd/compare/master...0xNoor:rtems-libbsd:master)

#### RTEMS:

[https://github.com/0xNoor/rtems/compare/master...0xNoor:rtems:noor-master](https://github.com/0xNoor/rtems/compare/master...0xNoor:rtems:noor-master)

### List of Obstacles:

#### Genet Drivers:

There was a lot of issues regarding this. Summarized as "*Encountered issues with outdated FreeBSD revision in RTEMS-libbsd, resolved by importing Genet drivers into rtemsbsd folder. Faced compilation errors; reverted to an earlier Genet patch from April 2020, made adjustments, and successfully compiled. Struggled with Genet driver recognition due to dependencies on simple-bus and FDT support*". But this was not all that. The libbsd being older actually created some problems. The `miifdt` was updated in the newer commit in the freebsd which is crucial for genet to work but manually updating created a lot of problems. I had to leave it as it is. So I'll need to wait till other better developers than me can take the updating part themselves.

#### Turning other cores on RPI using addresses:

Working of Raspberry Pi is weird (Not the first time by raspberry pi). With the latest firmware, only the primary core runs (core 0), and the secondary cores are awaiting in a spin loop. To wake them up, we need to write a function's address at 0xE0 (core 1), 0xE8 (core 2) or 0xF0 (core 3) and they will start to execute that function.

#### Changing Kernel Start section:

There are 4 address where the raspberry pi can start their kernel from. There addresses are

* 0x8000 for 32-bit kernels ("arm\_64bit=1" in config.txt not set)
    
* 0x80000 for older 64-bit kernels ("arm\_64bit=1" set, flat image)
    
* 0x200000 for newer 64-bit kernels ("arm\_64bit=1" set, gzip'ed Linux ARM64 image)
    
* 0x0 if "kernel\_old=1" set
    

Earlier RTEMS used the address 0x80000 for starting the RTEMS kernel, But as of writing this, the kernel is now being started from 0x0 because of the way how SMP is working in this configuration. This is a bit hacky way of making things works.

#### Understanding other examples for SMP:

There are a lot of ways where SMP has been implemented in other examples such as bare metal, RT-thread and other older example. And everyone had a different implemenation. It just a bit confusing to get a general idea on how they did it.

### Work done as of now:

#### SMP:

This is the most important task which I done so far, (Perhaps the most worthy too). Now RTEMS can utilize all the 4 cores of the Raspberry and unleash it's wrath upon thee Mortals. Haha Just kidding.

#### Libbsd driver integration:

The Raspberry pi is now compatible with all the drivers which are currenrtly available in the libbsd. However, most of them are redundant since the major USB and ethernet drivers are missing from the libbsd itself.

#### FDT support:

I have added support for DTB which is recognized by the ofwbus, but you know the sad part of this? The DTB won't be used anyway because the ethernet doesn't work. Tsk Tsk

### What's left to do?

The very first priority in this the Ethernet drivers, I'll be waiting for an update to the libbsd repo. After that, I assume it would be to look into Utakarsh's work on this BSP. He has done a great job for this BSP, but currently, his patches are on hold, not sure as of now. Merging all the patches would be great since they bring very crucial functions to the BSP.

That's all today, had a great time with my mentors. Adios everyone!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695570254526/099f551e-68f9-4715-bfd5-92d945a6c726.png align="center")