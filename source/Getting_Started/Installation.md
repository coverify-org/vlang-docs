---
seo:
  title: Installing Vlang
title: Vlang Installation
weight: 200
layout: page
navigation:
  show: true
---
## Prerequisites

### Operating System
{% info %}
Vlang is a *Domain Specific Embedded Languge*, built on top of the D programming language. Vlang on its own adds very little code that is specific to the operating system it is running on. As a result Vlang is easily portable to any Operating System that has a D compiler port.
{% endinfo %}

At this point Vlang works only on Linux platforms. But this is going to change soon since we are working on making Vlang compatible with Windows. We recommend running Vlang on a system with 4GB (minimum 2GB) RAM.

Vlang has been tested to compile and run on Ununtu 12.04 (and newer) and CentOS 6 (and newer) platforms.

### Required Software Packages

You need a working installation of the DMD 2.066 (or newer) compiler. Generic instrutions for installing the DMD compiler are avaiable at the D Programming Language [website](http://dlang.org).

The easiest way to install DMD compiler on any operating system is to download the compilers zipped package from its [download page](http://dlang.org/download.html). Then you need to unzip the package and add the desired bin directory to your execution path:
{% codeblock %}
$ cd ${HOME}/local
$ unzip ${HOME}/Downloads/dmd.2.066.1.zip
$ export PATH=${HOME}/local/dmd2/linux/bin64:${PATH}
{% endcodeblock %}

For DMD to work, you also need a working GCC compiler in yout PATH. Hopefully that is already installed on your system.

## Installation

{% info %}
For the moment, no Vlang release has been made and you need to download Vlang from Github. **You will need version control utility Git installed on your system**.
{% endinfo %}

Since you are installing a development version of Vlang, you only need to clone the Github repos:

{% codeblock %}
$ git clone https://github.com/coverify/vlang.git
$ export VLANGDIR=${PWD}/vlang
$ git clone https://github.com/coverify/vlang-uvm.git
$ export UVMDIR=${PWD}/vlang-uvm
{% endcodeblock %}
