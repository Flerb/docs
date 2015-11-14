---
author:
  name: William Dover
  email: docs@linode.com
keywords: 'ubuntu, irc, server, messaging'
license: '[CC BY-ND 3.0](http://creativecommons.org/licenses/by-nd/3.0/us/)'
modified: 
modified_by:
  name: 
published: ''
title: Installing UnrealIRCd on Ubuntu 15.10
external_resources:
- '[Understanding UNIX permissions and chmod](http://www.perlfect.com/articles/chmod.shtml)'
---

UnrealIRCd is a popular cross-platform Internet Relay Chat (IRC) Server. Providing the backbone for many IRC networks worldwide, it is well-regarded for its configurability and security features. 

{: .note}
>
>This guide is written for a non-root user. Commands that require elevated privileges are prefixed with `sudo`. If you're not familiar with the `sudo` command, you can check our [Users and Groups](/docs/tools-reference/linux-users-and-groups) guide.


## Prerequisites

* Ensure you have been through the [Getting Started](/docs/getting-started) guide.
* Make sure your server [is secure](/docs/security/securing-your-server)
* Make a rule to allow inbound traffic on port 6666:
    sudo iptables -A INPUT -p tcp --dport 6666 -j ACCEPT
* [GNU Screen](/docs/networking/ssh/using-gnu-screen-to-manage-persistent-terminal-sessions)
* A working IRC client


## Installation
### Download and Extract the Source
{: .note}
>
>You may be able to get a more recent version from the [UnrealIRCd website](https://www.unrealircd.org/download) but compatibility with this guide cannot be guaranteed, as it is only guaranteed to work with 3.2.10.5.tar.gz

1. Download the source: 
    
    wget  https://www.unrealircd.org/downloads/Unreal3.2.10.5.tar.gz 
2. Extract the source  - you should be able to press the tab key on your keyboard after typing the first few letters of the filename:

    tar zxvf Unreal3.2.10.5.tar.gz

### Configuration

1. Change the directory to `/Unreal3.2.10.5` by running `cd Unreal3.2.10.5`
2. Start the configuration script by running `./Config`.
3. When prompted, press Enter to continue.
4. Press enter until the Release Notes close.


You will then be asked some questions which determine how the server will be configured, a recommended answer will be given with each one:

    What directory are all the server configuration files in?

Press enter to this, in order to keep your configuration files in the UnrealIRCd folder.

    What is the path to the ircd binary including the name of the binary?

This tells UnrealIRCd where to look for the executable that it runs. Since this hasn't changed, press enter to keep the default.

    What should the default permissions for your configuration files be? (Set this to 0 to disable)

This should be set to 0600 - so only the owner can read and write. Further reading on exactly what this means is available in the External Resources section.









