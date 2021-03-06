---
title: Compilation And Installation
layout: page
---

If you want to compile the sources from a release tarball, you'll have to install development files for:

 * [libqb](https://github.com/ClusterLabs/libqb) - used for IPC
 * [libsodium](http://libsodium.org) - used for hashing
 * systemd-devel - used for udev

Optionally, you may want to install:

 * [libseccomp](https://github.com/seccomp/libseccomp) - used to implement a syscall whitelist
 * [libcap-ng](https://people.redhat.com/sgrubb/libcap-ng/) - used to drop process capabalities

And then do:

    $ ./configure
    $ make
    $ sudo make install

After the sources are successfully built, you can run the testsuite by executing:

    $ make check

If you want to compile the sources in a cloned repository, there are additional step required:

    $ git submodule init
    $ git submodule update

This will fetch the sources of [json](https://github.com/nlohmann/json/), [spdlog](https://github.com/gabime/spdlog) and [Catch](https://github.com/philsquared/Catch) which are used in this project too.

And to generate the *configure* script, run:

    $ ./autogen.sh

If you want to modify the lexer and/or the parser, you'll have to generate new source files for them. To learn how to do that, read [src/Library/RuleParser/README.md](https://github.com/dkopecek/usbguard/blob/master/src/Library/RuleParser/README.md).

## OS packages

### Fedora Linux, RHEL or CentOS

Pre-compiled packages for Fedora 21, 22, 23, rawhide and EPEL 7 (RHEL, CentOS) are distributes using a Copr [repository](https://copr.fedoraproject.org/coprs/mildew/usbguard/).
You can install the repository by executing the following steps:

    $ sudo yum install yum-plugin-copr
    $ sudo yum copr enable mildew/usbguard
    $ sudo yum install usbguard
    $ sudo yum install usbguard-applet-qt

### Gentoo

For Gentoo you can use the [stuge overlay](https://github.com/das-labor/overlay) via layman:

    $ layman -a das-labor
    $ emerge -av usbguard

### Usage

**WARNING**: before you start using usbguard be sure to configure it first unless you know exactly what you are doing (all USB devices will get blocked). For more information on how to configure usbguard see [configuration](https://usbguard.github.io/documentation/configuration.html) and [rule language](https://usbguard.github.io/documentation/rule-language.html).

To actually start the daemon, use:

    $ sudo systemctl start usbguard.service

You can interact with the daemon using either the CLI (see usbguard(1)) or the GUI Qt applet, which will also notify you about device events and ask for authorization of devices that don't match any rule in the policy. To start the applet, use the following command:

    $ usbguard-applet-qt
