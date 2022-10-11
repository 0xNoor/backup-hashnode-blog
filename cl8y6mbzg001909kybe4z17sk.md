# GSoC 2022 Final Submission: RTEMS port for Raspberry pi 4 AArch64

### Hey Everyone,
If you're reading this, That means I've completed my GSoC Project, Woohoo.
Here, I'm gonna describe what i've done in the past weeks. (not much, but some afterall I extended my timeline for a bit ;) So lets get started.

### Project goal
- To add a new aarch64 BSP to the RTEMS family.
- Making sure the BSP can perform various test and benchmarks

### Repos:

- This is where I've done all the work: 
https://Github.com/0xnoor/rtems/tree/noor-dev

- This is where I've written the documentation:
https://Github.com/0xNoor/rtems-docs/tree/noor-dev

### List of problems I faced:

I think this comes first here because before the discussion of what i've done because problems has better detail of progress than the actual work.

#### Finding Peripheral address:
In the Documentation of BCM2711, the address which they have given is based on when the raspberry pi is on high peripheral mode but I required low peripheral mode address instead. So that was a bit messy part.

#### Accessing CPU registers:
This was a lot troublesome as this was just the beginning for the development of the BSP so no console output was available. BUT then JTAG and rtems-exception-frame came to the rescue. It printed out every register state to the GDB.

#### Bringing up SMP

Alas, This is not implemented yet even though I've listed this to do this in the proposal. The problem with it was the Assembly code. I'm fairly novice to the assembly. TF-A maybe used to bring SMP support but I'm not sure as of now. 


### Details of work I've done:

#### Linker section:
This was just a little to hard to find the correct address from where the kernel starts in the raspberry pi 4, but I'm glad there was a guy who had the same doubt just some months ago. So here's the answer.

- 0x8000 for 32-bit kernels ("arm_64bit=1" in config.txt not set)
- 0x80000 for older 64-bit kernels ("arm_64bit=1" set, flat image)
- 0x200000 for newer 64-bit kernels ("arm_64bit=1" set, gzip'ed Linux ARM64 image)
- 0x0 if "kernel_old=1" set

#### Console Driver:
I've used the existing ARM PL011 drivers, nothing much to do here. Note that Mini-uart is not supported as of now.


#### Timer:
ARM generic timer, nothing to do here too

#### MMU Section:
They are currently composed of Peripheral address, local arm register, and GIC-400. 

### Testing the testsuites examples:
Let's test some benchmarks :)

#### Linpack 
```
...
norm resid      resid           machep         x[0]-1          x[n-1]-1
   1.9    8.46778499e-14   2.22044605e-16  -1.11799459e-13  -9.60342916e-14

Times are reported for matrices of order          100
1 pass times for array with leading dimension of  201

      dgefa      dgesl      total     Mflops       unit      ratio
    0.00195    0.00006    0.00202     340.60     0.0059     0.0360

Calculating matgen overhead
        10 times   0.00 seconds
       100 times   0.02 seconds
      1000 times   0.16 seconds
      2000 times   0.32 seconds
      4000 times   0.63 seconds
      8000 times   1.26 seconds
     16000 times   2.53 seconds
     32000 times   5.05 seconds
Overhead for 1 matgen      0.00016 seconds

Calculating matgen/dgefa passes for 5 seconds
        10 times   0.02 seconds
       100 times   0.21 seconds
       200 times   0.42 seconds
       400 times   0.84 seconds
       800 times   1.68 seconds
      1600 times   3.37 seconds
      3200 times   6.74 seconds
Passes used       2373 

Times for array with leading dimension of 201

      dgefa      dgesl      total     Mflops       unit      ratio
    0.00195    0.00006    0.00201     341.73     0.0059     0.0359
    0.00195    0.00006    0.00201     341.74     0.0059     0.0359
    0.00195    0.00006    0.00201     341.73     0.0059     0.0359
    0.00195    0.00006    0.00201     341.72     0.0059     0.0359
    0.00195    0.00006    0.00201     341.72     0.0059     0.0359
Average                               341.73

Calculating matgen2 overhead
Overhead for 1 matgen      0.00016 seconds

Times for array with leading dimension of 200

      dgefa      dgesl      total     Mflops       unit      ratio
    0.00194    0.00006    0.00200     342.62     0.0058     0.0358
    0.00194    0.00006    0.00200     342.64     0.0058     0.0358
    0.00194    0.00006    0.00200     342.64     0.0058     0.0358
    0.00194    0.00006    0.00200     342.62     0.0058     0.0358
    0.00194    0.00006    0.00200     342.63     0.0058     0.0358
Average                               342.63

Rolled Double  Precision      341.73 Mflops 

``` 



#### dhrystone
```
...
Microseconds for one run through Dhrystone:    0.2 
Dhrystones per Second:                      6381402.2 
DMIPS:                                      3631.99 
```

#### Whelstone
```
....
Loops: 10000, Iterations: 1, Duration: 0.337168 sec.
C Converted Double Precision Whetstones: 2965.9 MIPS


```



### What’s left to do
The project is complete even thogh its just a bare start with just minimum things require to interface with the Soc. Many of features are still remaining like GPIO, SPI, I2C, ethernet, USB and many more to count.


