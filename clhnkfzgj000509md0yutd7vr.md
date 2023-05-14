---
title: "GSoC with RTEMS Again..."
datePublished: Fri May 12 2023 15:22:27 GMT+0000 (Coordinated Universal Time)
cuid: clhnkfzgj000509md0yutd7vr
slug: gsoc-with-rtems-again
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1684076037665/24b73261-422a-4145-aeb3-7af63dbe395a.jpeg
tags: c, gsoc, rtos, rtems

---

Hello everyone, I'm Mohd Noor Aman, a 2nd-year Bachelor of Computer Applications student. As a second-time GSoC participant with RTEMS, I am thrilled to be back and eager to dive into my new project. Last year, I had the opportunity to work on porting aarch64 RTEMS for Raspberry Pi 4B, which was an incredible experience. This year, I am excited to continue my contributions to the RTEMS project with my new project, which involves implementing Ethernet and SMP support for the Raspberry Pi 4B. With the knowledge and experience, I gained from my previous project, I feel well-prepared to tackle this new challenge. I look forward to collaborating with my mentors from last year and learning and growing as a developer through this experience. Furthermore, I hope that my contributions will be valuable to RTEMS support for Raspberry Pi Family. I plan to document my progress through regular blog posts and engage with the community, and I am eager to see what the future holds for this project.

### A bit of a summary of what I plan to do this summer is:

* Ethernet Support: This will enable the BSP to connect to other devices or to the internet.
    
* SMP support: This will enable Multiprocessing support for RTEMS applications. One of the examples for using SMP is OpenMP, a popular parallel programming model used to improve the performance of scientific and engineering applications.
    

### Some of the hurdles which I might face during this might be:

* SMP implementation: Initially, I planned to implement SMP using the PSCI method with Trusted Firmware-A, similar to how it's done in Xilinx MPSoc. However, upon reviewing the TF-A documentation for Raspberry Pi 3, I noticed that instructions for patching the Linux kernel to use PSCI for SMP were absent for Raspberry Pi 4. Further investigation revealed that the current TF-A port for Raspberry Pi 4 is minimal, with only the armstub8.bin to bring up GIC available.
    
* Importing Drivers from Freebsd: It should be a fairly easy task, but for those who have already worked with the RTEMS-libbsd repo. I'm a new timer, it'll take a bit of time to understand all of that
    

I hope i'll be upcoming over every obstacle with the help of my mentors. Wish me luck, very much needed **;)**