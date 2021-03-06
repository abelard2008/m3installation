EtherCAT driver for a non-supported kernel
=====

The Meka robot at Ensta uses a combination of [Rtai](https://www.rtai.org/) + [EtherCAT](http://etherlab.org/en/ethercat/) to control the motors.

> Update 22 august 2014

## Introduction
* The ethercat master 1.5.2 for linux [only supports a specific set of kernels](http://etherlab.org/en/ethercat/hardware.php) (2.6.x, 3.2, 3.4) with specific Ethernet drivers : the one on the meka is **r8169**, which is supported on almost all listed kernels.

* Rtai also supports a limited set of kernels (3.4.67, 3.5.7, 3.8.13 for the official rtai-4.0), so we need to build a kernel supported by both Rtai and Ethercat master.

## Kernel 3.4.67

The only common kernel here is **3.4.67**.

> **Issues** :
* _(Major)_ Crashes occur when the basic rtai tests are launched (in /usr/realtime/testsuite/kernel/switches).
* _(Medium)_ isolcpus (kernel parameter) and IsolCpusMask (rtai_hal.ko parameter) hangs the system.
* _(Minor)_ The PC used for realtime on the meka is equipped with an i7-3770S Sandy Bridge, with integrated HD graphics 3000, which are NOT supported on 3.4. All the computation is software, i.e slow and creates a lot of interruptions. 

## Kernel 3.8.13

This is the last rtai 4.0 officially supported kernel.
we need to build the ec_r8169.ko module for 3.8.13


## Kernel 3.10.32

We have to use the [CVS magma version](http://cvs.gna.org/cvsweb/magma/?cvsroot=rtai) (latest build). Support for 3.10.32 has been added in may 2014.

we need to build the ec_r8169.ko module for 3.10.32

go to linux-3.10.32/drivers/net/ethernet/realtek

and copy r8169.c to ethercat-1.5.2/devices/r8169-3.10-ethercat.c
and ethercat-1.5.2/devices/r8169-3.10-orig.c

now open meld and diff the closest previous ethercat version (3.4 for example).

Now add to r8169-3.10-ethercat.c the 'ethercat' functions added.

