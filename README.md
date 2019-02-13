
# Adaptive RED for FreeBSD
This project includes code for Adaptive RED - an Active Queue Management algorithm - in FreeBSD altq.

## Getting Started  
### Prerequisites

Our code is based on `FreeBSD-CURRENT`. In order to download the source, please checkout the [FreeBSD guide](https://www.freebsd.org/doc/handbook/bsdinstall.html).

### Directory Structure
```
.
+-- net
|   +-- altq 
|       +-- altq_red.h
|       +-- altq_red.c
+-- conf
|   +-- options
+-- README.md
+-- LICENSE.md
```
### Installation
#### Replace files
* Replace the /net/altq/altq_red.h file with the one provided.
* Replace the /net/altq/altq_red.h file with the one provided.
* Replace the /net/conf/options files with the one provided.

#### Compile and install AdaptiveRED kernel
* Create a custom kernel file called ALTQ_KERNEL in the /sys/amd64/conf directory with the following lines-
```Bash
   include GENERIC
   ident ALTQ_KERNEL
   options IPFIREWALL
   options DUMMYNET
   options ALTQ
   options ALTQ_RED
   options ALTQ_ADAPTIVE_RED
```
* In the /usr/src directory, enter the following command to build the kernel-
```Bash
   make buildkernel KERNCONF=ALTQ_KERNEL
```
* Install the custom kernel using 
```Bash
   make installkernel KERNCONF=ALTQ_KERNEL
```
### Usage
When the kernel is built with the ALTQ_ADAPTIVE_RED parameter set, Adaptive RED is used by default instead of RED. A sample pf ruleset to enable RED will be:
```bash
ext_if="em2"
int_if="em1"

altq on $ext_if hfsc bandwidth 20Mb queue {def_up,red_up}
queue def_up bandwidth 50% hfsc(default)
queue red_up bandwidth 50% hfsc(red)

pass from any to any
pass out from any to any queue red_up
```
## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Acknowledgments
This project was carried out as part of the [Wireless Information and Networking Group, NITK](http://wing.nitk.ac.in/) under the supervision of [Prof. Mohit P. Tahiliani](https://github.com/mohittahiliani).
