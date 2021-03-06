
### Description

GNURadio-NoVNC is both a HTML VNC client JavaScript library, it us noVNC Client to runs well in any modern browser including mobile browsers (iOS and Android) to use and provide accessiblity to GNURadio for remote access development over CRC Testbed.

### NoVNC Features

* Supports all modern browsers including mobile (iOS, Android)
* Supported VNC encodings: raw, copyrect, rre, hextile, tight, tightPNG
* Supports scaling, clipping and resizing the desktop
* Local cursor rendering
* Clipboard copy/paste
* Licensed mainly under the [MPL 2.0](http://www.mozilla.org/MPL/2.0/), see
  [the license document](LICENSE.txt) for details

### Screenshots

Running in Firefox before and after connecting:

<img src="https://github.com/astro7x/GNURadio-NoVNC/blob/master/img/login.png" width=400>;
<img src="https://github.com/astro7x/GNURadio-NoVNC/blob/master/img/connected.png" width=400>

### Browser Requirements

noVNC uses many modern web technologies so a formal requirement list is not available. However these are the minimum versions we are currently aware of:
* Chrome 49, Firefox 44, Safari 10, Opera 36, IE 11, Edge 12


### Server Requirements

noVNC follows the standard VNC protocol, but unlike other VNC clients it does require WebSockets support. Many servers include support (e.g. [x11vnc/libvncserver](http://libvncserver.sourceforge.net/),
[QEMU](http://www.qemu.org/), and [MobileVNC](http://www.smartlab.at/mobilevnc/)), but for the others you need to use a WebSockets to TCP socket proxy. noVNC has a sister project [websockify](https://github.com/novnc/websockify) that provides a simple such proxy.


### Installation Requirements

Install the server side, TightVNC which is a remote desktop control software that enables remote accessibility. To install, use the following yum command as shown below.
``` 
 yum -y install tigervnc-server xorg-x11-fonts-Type1
```
```
 yum --enablerepo=epel -y install novnc python-websockify numpy 
```
``` 
vncserver :1
```

### Quick Start

* Download current Repo
``` bash
git clone https://github.com/astro7x/GNURadio-NoVNC
```
* Use the launch script to automatically download and start websockify, which
  includes a mini-webserver and the WebSockets proxy. The `--vnc` option is
  used to specify the location of a running VNC server:
```
./utils/launch.sh --vnc localhost:5901
```

* Point your browser to the cut-and-paste URL that is output by the launch   script. Hit the Connect button, enter a password if the VNC server has one configured, and enjoy!

### TODO
create a script to create random users to use the GNURadio-NoVNC service for a fixed timeslot and terminate/abort the connection after time slot duration.

## Running the server for multiple users
* Create 2 users and then set their password accordingly
``` bash
adduser vncuser1 vncuser2 
```
* Edit the /etc/sysconfig/vncservers file, by adding the following to the end of file
``` bash
VNCSERVERS="1:vncuser1 2:vncuser2"
VNCSERVERARGS[1]="-geometry 1024x600
VNCSERVERARGS[2]="-geometry 1024x600
```

* The iptables rules need to be modified to open the desired VNC ports, e.g.
```
sudo iptables -I INPUT 5 -m state --state NEW -m tcp -p tcp -m multiport --dports 5901:5903,6001:6003 -j ACCEPT
sudo service iptables save
sudo service iptables restart
```

* Make sure that .vnc/xstartup exists for both users (vncuser1 and vncuser2) and edit the end of script to contain:
```
#twm & 
exec gnome-session &
```
* Restart The vncserver
```
sudo service vncserver restart
```
* To start the session for vncuser1:
```
vncserver :1
./utils/launch.sh --vnc localhost:5901 --listen 6080
```
* To start the session for vncuser2:
```
vncserver :2
./utils/launch.sh --vnc localhost:5901 --listen 6081
```

[REF](https://www.howtoforge.com/vnc-server-installation-on-centos-7)


### NoVNC Project Reference
[NOVNC](http://novnc.com)


