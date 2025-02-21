# Connecting to CRC Servers

All activity performed on the CRC infrastructure occurs on remote servers, this requires an `ssh` (Secure Shell) connection to a head node at the very least. Below are instructions for creating an `ssh` session to a head node specific to each popular OS.

For more information on the Public head nodes see the `front_end_workflow` page.

!!! note
    For users connecting from off campus, please review the section `off-campus-connect` first.

------------------------------------------------------------------------

## Connecting from Linux

Most distributions of Linux are shipped with a terminal available. Most likely, `ssh` will already be installed. Open a terminal, the process for doing so can very from distro to distro. Popular key-bindings are "CTRL + Alt + T", or "Alt + F2". From here you can simply use the `ssh` command to connect:

``` shell
ssh netid@crcfe01.crc.nd.edu
```

------------------------------------------------------------------------

### X11 Forwarding

If you plan on using a GUI (Graphical User Interface), you will need to forward an X connection to our servers. To do so, use the `-Y` flag to pass trusted X11 connections.

``` shell
ssh -Y netid@crcfe01.crc.nd.edu
```

------------------------------------------------------------------------

## Connecting from a Mac

As macOS is a form of `Unix`, there is an included terminal application. Open the Launchpad and search for "Terminal". Once you open the "Terminal" program you will have a shell in front of you. From here, you can use the `ssh` command.

``` shell
ssh netid@crcfe02.crc.nd.edu
```

------------------------------------------------------------------------

### X11 forwarding for Mac

If you plan on using a GUI (Graphical User Interface), you will need to first download a program called `XQuartz`. Go to the [XQuartz download page](https://www.xquartz.org/). It will install like most other macOS applications.

Once `XQuartz` is installed, you can use the `-Y` flag to `ssh` within a terminal to connect to a head node.

``` shell
ssh -Y netid@crcfe01.crc.nd.edu
```

!!! note
    Development of `XQuartz` has lagged quite behind macOS. A number of issues with it has been reported to us. It may be easier to use `fastx` for Mac users.

------------------------------------------------------------------------

## Connecting from Windows

For all versions of Windows, we recommend using MobaXterm. Detailed instructions on how to connect from Windows see the `mobaxterm` page.

------------------------------------------------------------------------

## Connecting from off campus

If you are connecting from off campus or unable to use the EDUROAM wireless network, you will first need to connect to the campus VPN. Installation instructions for Windows, Mac, and Linux are available from this OIT hosted site:

<https://nd.service-now.com/nd_portal?id=product_page&sys_id=9d5919c7db22a34099dcf25bbf9619e2&table=cmdb_ci_business_app/>

If you are unable or prefer not to use the campus VPN, one alternative is to use the server `bastion.crc.nd.edu` as your login host.
