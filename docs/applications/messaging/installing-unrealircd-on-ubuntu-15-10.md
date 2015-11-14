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
---

UnrealIRCd is a popular cross-platform Internet Relay Chat (IRC) Server. Providing the backbone for many IRC networks worldwide, it is well-regarded for its configurability and security features. 

## Prerequisites

* Ensure you have been through the [Getting Started](/docs/getting-started) guide. 
* Make sure your server [is secure](/docs/security/securing-your-server).
* Make a rule to allow inbound traffic on port 6666:
    sudo iptables -A INPUT -p tcp --dport 6666 -j ACCEPT
* [GNU Screen](/docs/networking/ssh/using-gnu-screen-to-manage-persistent-terminal-sessions)


## Download and Installation
### Download and Extract the Source
{: .note}
>
>You may be able to get a more recent version from the [UnrealIRCd website](https://www.unrealircd.org/download) but compatibility with this guide cannot be guaranteed, as it is only guaranteed to work with 3.2.10.5.tar.gz

1. Run `wget  https://www.unrealircd.org/downloads/Unreal3.2.10.5.tar.gz` to download the source.
2. Run `tar zxvf Unreal3.2.10.5.tar.gz` - you should be able to press the tab key on your keyboard after typing the first few letters of the filename.

### Configuration


