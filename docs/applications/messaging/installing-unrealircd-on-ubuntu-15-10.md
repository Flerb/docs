---
author:
    name: Alex Fornuto
    email: docs@linode.com
description: 'Using the ZNC bouncer to retain an IRC connection.'
keywords: 'znc,irc,debian,source,debian 8,debian 7,messaging,chat'
license: '[CC BY-ND 3.0](http://creativecommons.org/licenses/by-nd/3.0/us/)'
modified: Thursday, June 4th, 2015
modified_by:
    name: 'Elle Krout'
published: 'Friday, August 21, 2014'
title: 'Installing ZNC from Source on Debian'
---

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
* Ensure you have installed `build-essential`:

    
    apt-get install build-essential


## Installation
### Download and Extract the Source
{: .note}
>
>You may be able to get a more recent version from the [UnrealIRCd website](https://www.unrealircd.org/download) but compatibility with this guide cannot be guaranteed, as it is only guaranteed to work with 3.2.10.5.tar.gz

1. Download the source: 
    
    `wget  https://www.unrealircd.org/downloads/Unreal3.2.10.5.tar.gz`

2. Extract the source  - you should be able to press the tab key on your keyboard after typing the first few letters of the filename:

    `tar zxvf Unreal3.2.10.5.tar.gz

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

    Do you want to support SSL (Secure Sockets Layer) connections?
    
This question is asking whether you want SSL to be supported. SSL, while allowing a secure connection, would require you to obtain a certificate. Certificates generally need to be signed by a trusted third-party, and this can be expensive and time consuming, so it is advised that you press Enter to make it use the default of no SSL.

    Do you want to enable IPv6 support? 
    
This guide assumes the default no has been used, but some readers may wish to make use of their IPv6 addresses. 

    Do you want to enable ziplinks support?
    
`ziplinks` is intended to aid communication between networks running more than one server. Only one is being set up here, so press Enter to accept the default no.

    Do you want to enable remote includes?
    
This would allow you to use the same config file across different servers. Again, we are only running one server here and as such this isn't needed. Press Enter to accept the default of no.


    Do you want to enable prefixes for chanadmin and chanowner?
    
Enabling prefixes will enable the easier identification of administrators in channels, so press Enter to acccept the default of yes.

    What listen() backlog value do you wish to use?  Some older servers
    have problems with more than 5, others work fine with many more.
    
The listen() backlog value gives the number of connections that can be queued at a time. This option is not particularly important, as not many will likely need to be queued but your server should be more than capable of a much higher number than five. Press Enter to set it at five, or optionally increase it if you are expecting to have many people trying to connect at once.

    How far back do you want to keep the nickname history?

The server supports a command called `/whowas` which tells the client what usernames a given user has used in the past. 2000 is the default. This is more than enough as most users will seldom change their username and those who do are unlikely to do it more than 2000 times. 


    What is the maximum sendq length you wish to have?
    
This is the amount of bytes that the server can have queued to be sent before the connection is closed. The default 3000000 is plenty. Press Enter to accept it.

    How many buffer pools would you like?
    
Keeping this setting at the default 18 should keep memory usage down, press Enter to accept this.


    How many file descriptors (or sockets) can the IRCd use?
    
This determines how many connections can be open at a time. Most servers will never exceed the default of 1024, if you are expecting more than 1024 connections, then increase this option accordingly, otherwise just press Enter to accept the default.

    Would you like to pass any custom parameters to configure?
    
This would allow you, if you wanted, to define custom options for the configuration. Press enter to accept the default of none.

UnrealIRCd should now set these up. This may take a while. When it is done, move on to the next section.


### Compilation

The compilation is straightforward:

    make
    
When you get a message indicating compilation is complete, move on to the next section.

## Configuration

UnrealIRCd expects a configuration file called `unrealircd.conf`. There is an example stored in the doc directory under the directory you extracted UnrealIRCd into. Copy it to the main directory:

    cp ~/Unreal3.2.10.5/doc/example.conf ~/Unreal3.2.10.5/unrealircd.conf
    
Open it with nano:

    nano unrealircd.conf

Scroll down to:

{: .file-excerpt}
Unreal3.2.10.5/unrealircd.conf
~~~ 
/* FOR *NIX, uncomment the following 2lines: */
//loadmodule "src/modules/commands.so";
//loadmodule "src/modules/cloak.so";
~~~
Uncomment said lines by removing the two forward slashes at the start of the lines.

Next, scroll down to:

{: .file-excerpt}
Unreal3.2.10.5/unrealircd.conf
~~~ 
me
{
        name "irc.foonet.com";
        info "FooNet Server";
        numeric 1;
};
~~~

Edit this to match your hostname - `name` should be the full domain name of your server, `info` should just be the name of your server. `numeric` should be left at one, since this is a single-server network.


Next, scroll down to the admin info settings:

{: .file-excerpt}
Unreal3.2.10.5/unrealircd.conf
~~~
admin {
        "Bob Smith";
        "bob";
        "widely@used.name";
};
~~~

You can provide as many lines of contact details for yourself as you wish, but you are advised not to use a personal email address as it might get subjected to spam.


