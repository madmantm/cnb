============================
The Chuck Norris Bot Project
============================

The Chuck Norris Bot (CNB) project is a multi-protocol chat bot. It currently
support XMPP/Jabber and IRC. 

The concept is that the bot has a single "brain" but connect to multiple
servers at once so beside responding to simple bot tasks such as saying
hello, launching nmap, replying Chuck Norris facts and more, the bot can do
some correlated tasks such as outputing text from an IRC channel to a XMPP room.


Prerequisite
============
The bot depends on the following packages:

* python-irclib (LGPL v2.0)
* python-xmpp  (GPL v2.0)
* python-pywapi (MIT)
* python-httplib2 (MIT)
* python-libxml2 (MIT)
* python-django (BSD)
* nmap 

The XMPP/Jabber connector was tested on ejabberd and GTalk.
The IRC connector was tested on freenode.net.
The bot was tested on Debian Stable (squeeze atm).

To install all dependencies, run the following script:
    ./cnb/bin/misc/inst-dep.sh

Install
=======

First download the code from git
    git clone https://github.com/hackfestca/cnb cnb

Then link the python code (python package is not done yet)
    ln -s cnb/cnb/ /usr/lib/pymodules/python2.6/

Then, change the ROOT_DIR var in cnb/bin/cnb-cli at line 32 to your own root

Finally, create a gmail account and/or freenode account for the bot


How to use
==========

Setting up the main config file
-------------------------------

First, go to *config/* folder and open the *cnb.conf*. This is the main config
file. Most of this should not have to change, except for the **connectors** and 
**smtp** sections. Here is an example of a *cnb.conf* file. Simply fill the
<...> fields.
    [global]
    #root-dir = <string>  (dynamically added)
    #bin-dir = <string>  (dynamically added)
    #config-dir = <string> (dynamically added)
    #log-dir = <string>  (dynamically added)
    #tp-dir = <string>  (dynamically added)
    #pid-file = <string>  (dynamically added if started as a daemon)
    version = 0.20
    log-format = %(asctime)s - %(name)s - %(levelname)s - %(message)s
    
    [connectors]
    auto = [freenode.irc.conf, gmail.xmpp.conf]
    
    [smtp]
    smtp-user = <an email address>
    smtp-pass = <a password>
    smtp-host = <a smtp server>
    smtp-port = <a smtp port>

As you can see in the **connectors** section, there are two more config files. 
These filse contain all necessary information to connect to a chat 
server. The next section explain how to setup a connection config file. 


Setting up a connection config file
-----------------------------------

The bot will import specified *\*.conf* files in *cnb.conf* file. Here's
the syntax of an IRC connection file. Again, simply fill the <...> fields. 
    [bot]
    type = irc
    log-file = freenode.irc.log
    username = <an irc username>
    password = <a password>
    server = <an irc server>
    channels = [<a list of chan to connect (Syntax: chan:[password],...)>]
    auto-join = 1
    auto-start = 1
    auto-reconnect = 1
    verbose = 0
    admins = [<a list of admins (Syntax: nick1,...). WARNING: THIS IS NOT SECURE>]

And this is a XMPP connection file
    [bot]
    #id = <int> (dynamically added)
    #config-file = <string> (dynamically added)
    #monday-suck-room = <string> (dynamically added)
    type = xmpp|xmpp-gtalk  //xmpp for custom xmpp, xmpp-gtalk for gmail chat
    log-file = gmail.xmpp.log
    username = <insert username here>
    password = <insert password here>
    server = <overwrite only if the server can't be resolved from SRV lookup.
    See <http://tools.ietf.org/html/rfc6120#section-3.2.1> >
    rooms = [<a list of default rooms to join (Syntax: room1,...)>]
    nickname = <insert nick name here>
    auto-join = 1
    auto-start = 1
    auto-reconnect = 1
    verbose = 0
    admins = [<a list of admins (Syntax: email1,...)>]

Running the bot
-----------------
It is recommended to start it as a shell script first to see errors if any 
and then start it as a service

To run the bot as a shell script:
    ./cnb/bin/cnb-cli [--help]

To run as a service:
    # Create a user with limited privileges    
    adduser chuck
    
    # Start the bot
    sudo ./cnb/bin/cnb-service start|stop|restart|status


Bot Security
============

Some principle
--------------

* Never run the bot as root
* For long time use, jail it on a VM
* Set up admin list correctly
    * You don't want anybody to run nmaps from your home?

Bot Hardening
-----------------

By default, running Chuck as a service will run it as the user "chuck". It 
is always a good idea to run the bot as a user with limited privileges.

Disabling modules can also reduce attack vectors. Disable modules by removing 
symbolic links in the cnb/modEnabled folder (apache style).


FAQ
===

Python can't find cnb package?
------------------------------

Ensure that the symbolic link is added in a standard path. On debian, I had the following:
    >>> import sys
    >>> str(sys.path)
    "['', '/usr/lib/python2.6', '/usr/lib/python2.6/plat-linux2', 
    '/usr/lib/python2.6/lib-tk', '/usr/lib/python2.6/lib-old', 
    '/usr/lib/python2.6/lib-dynload', '/usr/local/lib/python2.6/dist-packages', 
    '/usr/lib/python2.6/dist-packages', '/usr/lib/python2.6/dist-packages/PIL', 
    '/usr/lib/python2.6/dist-packages/gst-0.10', '/usr/lib/pymodules/python2.6', 
    '/usr/lib/pymodules/python2.6/gtk-2.0']"


Contributors
============
This bot was created by Martin Dubé as a Hackfest Project (See:
<http://hackfest.ca>). Martin is still the main collaborator and reviser but 
a lot of ideas came from Hackfest crew and community.

For any comment, questions, insult: martin d0t dube at hackfest d0t com. 

Thanks also to
--------------
Authors and maintainers of the following projects, which make this bot fun and
useful:
* findmyhash
* nmap
* eliza
* Trivia Game (vn at hackfest d0t ca)
* Bree Olson
* Python
* And every project I forgot


