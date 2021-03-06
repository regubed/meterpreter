Current `master` branch: ![Meterpreter Build Status](http://r7jenkins.cloudapp.net/Meterpreter "Meterpreter Build Status")

meterpreter >
=============

This is the new repository for the Meterpreter source, which was originally in the
[Metasploit Framework](https://github.com/rapid7/meterpreter) source.

Building - Windows
==================

Meterpreter is now being built with [Visual Studio 2012 Express for Desktop][vs_express] or any
paid version of [Visual Studio 2012][vs_paid]. Earlier toolsets on Windows are no longer
supported.

Visual Studio 2012 requires .NET 4.5 in order to run, and as a result isn't compatible
with Windows XP due to the fact that .NET 4.5 will not run on Windows XP. However, this
does not mean that Metepreter itself will not run on Windows XP, it just means that it's
not possible to _build_ it on Windows XP.

Visual Studio 2012 Express
--------------------------

In order to build successfully with this version of Visual Studio you must first make sure
that the most recent updates have been applied. At the time of writing, the latest known
update is **Update 3**. Without this update you won't be able to build.

To make sure you have the appropriate updates applied:

1. Open Visual Studio 2012.
1. Open the `Tools` menu and select `Extensions and Updates`
1. Select the `Updates` item on the left side of the dialog box.
1. Follow the prompts to install any updates that are found.

With those updates applied you should be ready to build Meterpreeter.

Running the Build
-----------------

Open up a Visual Studio command prompt by selecting `Developer Command Prompt for VS2012`
from the Start menu. Alternatively you can run `vcvars32.bat` from an existing command
line prompt, just make sure it's the VS2012 one if you have multiple versions of VS
installed on your machine.

Once you have your environment variables set up, change to the root folder where the
meterpreter source is located. From here you can:

* Build the x86 version by running: `make x86`
* Build the x64 version by running: `make x64`
* Build both x86 and x64 versions by running: `make`

The compiled binaries are written to the `output/x86` and `output/x64` folders.

If you are not a Rapid7 employee, make sure you build the source using the `debug` or
`release` configurations when inside Visual Studio. If you attempt to build `r7_debug` or
`r7_release` you will get compiler errors due to missing libraries.

If you build the source from the command line the toolset will choose the most
appropriate build configuration for you and hence calling `make` should "Just Work&trade;".

If you are a Rapid7 employee you will need the PSSDK source in order to build the
extra components using the `r7_*` build configurations.

Building - POSIX
================
You will need:
 - A compiler toolchain (build-essential package on Ubuntu)
 - gcc-multilib, if you're building on a 64-bit machine
 - jam
 - wget

Meterpreter requires libpcap-1.1.1 and OpenSSL 0.9.8o sources, which it
will download automatically during the build process. If for some
reason, you cannot access the internet during build, you will need to:
 - wget -O posix-meterp-build-tmp/openssl-0.9.8o.tar.gz http://openssl.org/source/openssl-0.9.8o.tar.gz
 - wget -O posix-meterp-build-tmp/libpcap-1.1.1.tar.gz http://www.tcpdump.org/release/libpcap-1.1.1.tar.gz

Note that the 'depclean' and 'really-clean' make targets will *delete*
these files.

Now you should be able to type `make` in the base directory, go make a
sandwich, and come back to a working[1] meterpreter for Linux.

[1] For some value of "working."  Meterpreter in POSIX environments is
not considered stable.  It does stuff, but expect occasional problems.


Testing
=======

There is currently no automated testing for meterpreter, but we're working on it.

Once you've made changes and compiled a new .dll or .so, copy the
contents of the output/ directory into your Metasploit Framework's
`data/meterpreter/` directory. In POSIX you can do this automatically if
metasploit-framework and meterpreter live in the same place by running
`make install`

If you made any changes to `metsrv.dll` or `msflinker_linux_x86.bin`, ensure
that all extensions still load and function properly.

  [vs_express]: http://www.microsoft.com/visualstudio/eng/downloads#d-2012-express
  [vs_paid]: http://www.microsoft.com/visualstudio/eng/downloads#d-2012-editions

