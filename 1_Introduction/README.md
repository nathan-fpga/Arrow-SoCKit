# Designing with the Arrow SoCKit: Part 1 (Introduction)

## Table of contents
1. [Introduction](#introduction)
2. [What is an FPGA?](#fpga-def)
3. [FPGA Families](#fpga-families)
4. [SOC FPGAs](#soc)
5. [Conclusion](#conclusion)

## Introduction
This series of projects will be an introduction to the Arrow SoCKit, one of my personal favorite Altera development boards. There are several things I prefer about the SoCKit to the more common Cyclone V development boards like the DE10-Standard, Lite and Nano, or the DE0 – there will be more exposition on this later in this series.

The SoCKit can be hard to find, as it went out of production a long time ago. Terasic may still have a few copies, but they are not generally in stock. That said, it should require minimal effort to port any of the designs in this series to a DE10 nano. In fact, one of the entries in this series will be an explicit port of one of my SoCKit designs to a DE10-Nano.

I hope to cover several major areas of interest in this series:

1. Architecture and the basics of Cyclone V development boards
2. The toolchain used to develop for Intel/Altera FPGAs
3. RTL development for Intel Cyclone V devices
4. Avalon IP and Platform Designer
5. SoC development and the HPS
6. Boot flow and basic software development.

There are many reasons why I'm focusing on Altera devices rather than Xilinx devices in this series. The primary reason is that the Xilinx design flow is far more explored – there are many excellent tutorials for Xilinx devices, such as Adam Taylor's excellent Microzed Chronicles for the low level implementation details of Zynq design, or many interesting application projects, among many other extensive series on the topic.

Altera, on the other hand, you have to dig around Rocketboards to find what you need. I am hoping to remedy that in this series to an extent.

## What is an FPGA? <a name="fpga-def"></a>
When discussing "FPGAs", people often conflate different things. An actual FPGA is shown below:

![fpgabga](https://user-images.githubusercontent.com/124276754/217410213-836e8926-d919-4f33-bea7-def9579914c4.png)

As you can see, the actual FPGA is an integrated circuit, IC. Modern FPGAs are almost invariably packaged as Ball-Grid-Array (BGA) packages due to the large number of pins. Typical FPGA BGAs are square, resulting in pin numbers such as 896 (30x30, including 4 corners) or 672 (26x26 including 4 corners).

There are multiple different types of pins. This article will not go into detail, but suffice it to say pins can either be power pins, ground pins, fixed-purpose pins (which have a specific function) or pins which the user can customize. Perhaps later we will have a chance to explore this more.

The boards people use to design with are colloquially referred to as FPGAs, but they are more accurately called development boards (Xilinx), development kits (Altera), or evaluation boards (when they use the expensive chips). All of these mean the same thing. They are platforms which integrate the FPGA chip into a more user-friendly environment, connecting the pins to all sorts of peripherals the user can use to interact with their digital logic. The next article will go over common peripherals, and the specific options provided on the Arrow SoCKit.

![image](https://user-images.githubusercontent.com/124276754/217410387-a8a3dcbf-828f-4147-a503-ad86a6f01d51.png)

The downside of development boards is that they limit the users' options. An FPGA chip can be used to interface with anything the user desires. On a development board, however, the vast majority of the FPGA pins are already committed to certain interfaces, allowing the user much less creativity. In exchange, though, the user does not have to design a PCB to use the FPGA this way, a tremendous advantage for accessibility and ramping up during the design process. (There are dev boards that provide various forms of customizable IO pins from different types of headers, but that's a topic for another day).

## FPGA Families <a name="fpga-families"></a>
FPGAs come in many different flavors and price ranges. There are FPGA families designed for specific applications as well (such as the Xilinx RFSoC). The primary breakdown is by price – FPGAs come in low-range families, mid-range families, high-end families, and there is a wide range of prices in the high-end families.

Make sure to understand the difference between a _family_ and the base technology. A Cyclone V and Stratix V belong to different families, but both use the same 28 nm technology. A Cyclone 10 uses 20 nm technology.

_Low-End_ FPGAs are relatively cheap, accessible devices. These are used for hobbyist projects, as well as lightweight industry applications that don't require large amounts of bandwidth or logic resources. The normal range for these is around $50-$500 for the FPGA chip itself, or $150-$1000 for a development board with one of these FPGAs. The Cyclone V that is the focus of this series is a good example of these.

_Midrange_ FPGAs straddle the line between the cheap FPGAs and the high-end families. These are usually used for substantial but not overly intensive business applications. They provide a lot of value for applications that need meaningfully advanced technology but not necessarily the cutting edge. Typically, the chips will cost $500-$3000. Development boards usually run from $1500 all the way up to $6k-7k.

_High-end_ FPGAs are the tools used for the most intensive applications. These FPGAs operate at much higher clock speeds than the cheap ones, contain many times the amount of logic, and integrate advanced Gigabit Transceivers (GBTs) for the highest data transfer speeds. These easily cost several thousand dollars, but can get a lot more expensive than that.

## SoC FPGAs <a name="soc"></a>
SoC stands for System-on-Chip. An SoC FPGA is an FPGA which integrate an ARM processor into the chip itself. This architecture allows for substantially higher bandwidth and _much_ more reliable communication between the programmable logic and the processing system. I have personal experience with a custom-designed FPGA-to-CPU bus. It was both slow and unreliable, and was extremely hard to debug. SoC interfaces can be debugged using the normal tools, as we will discuss later on.

The main motivation for this kind of chip is that CPUs and FPGAs are good at different things. Programmable logic excels at fast parallel processing – processing a lot of data at the same time. CPUs are optimized for _sequential_ programming, meaning executing a number of different tasks in order. By providing a fast, effective interface between the two you get the best of both worlds. This is an oversimplification obviously – there's a lot to learn about this topic if you want to.

The CPU on most hobbyist development boards, including the SoCKit, consists of a dual-core Arm Cortex A9, an ARMv7 CPU with 2 cores. ARMV7 is a 32-bit architecture.

Higher end chips like a Stratix 10, Agilex, Zynq MPSoC or Virtex-7 part usually are built on Arm Cortex A53s, a quad-core ARMv8 64-bit CPU. (There are several affordable kits from AMD-Xilinx built on the Zynq MPSoC with a quad-core A53, but those are not the focus of this series. Notable examples are the Kria KV/KR260 and the Ultra96V2.).

## Conclusion
In the first part of this series, we went over the fundamentals of what the Arrow SoCKit is. Specifically:
- What FPGAs and development boards are
- A general description of FPGA price ranges and where the Cyclone V fits in
- The concept of a System-on-Chip
- CPU architectures on SoCs

In the next part, we will discuss common development board peripherals, and how you should know which development board is right for you.

Hope you enjoyed the first part of this series! Feel free to send any comments, questions or suggestions to me at nathan.fpga@gmail.com
