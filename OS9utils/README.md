Decided to split the OS level utilities away from the web browser. An so, here are notes on ifup, ifconfig, host and stuff like that. 

First step, install frobio https://github.com/strickyak/frobio/.  Check out the 'built' directory! Copy to /dd/CMDS/f.dig, etc and use 'attr filename e pe' to make them all runnable. Second step, load and pack each of the basic09 programs individually, they will end up in /dd/CMDS. And finally, copy the example interfaces, hosts, and resolv.conf files to /dd/SYS and edit as described below. 
 

<b>host name.domain.tld</b>
The start of a local dns cache, host.b09 is recursive, it first checks /dd/SYS/tempdns.txt for preexisting work. If the earlier result includes a CNAME, but includes no A record, host does another f.dig and restarts. For now, we assume eventually there will be an A record, parser work needed (SOA, etc will hang).  In host.b09, set debug:=TRUE to watch the lookups roll by (3 in this case). 

But provided we eventually found an A record, the IP and original hostname (not the CNAMEs) are appended to /dd/SYS/hosts. We should return the IP to the caller, but don't yet.


<b>ifconfig</b>
Not really, but if the chip is configured it prints the basic IPv4 config data it's configured with. Do this while nothing else is happening. Lots of changes coming soon, will document ifconfig once it does more.


<b>ifup</b>
Uses /dd/SYS/interfaces, only 1 interface for now. It supports the Linux trick of setting a full static interface in the file, then change the one word 'static' to 'dhcp' when traveling. Free parking!

<b>Config Files</b>

<b>/dd/SYS/interfaces</b> is just that, a list of interfaces for ifup to bring up. Each one must have a name in the range eth0-eth9. There can be 2(RSN) CoCoIO using encapsulation 'inet' but other interfaces will be supported where we can. Finally, 'static' or 'dhcp'. If dhcp, the subsequent address data is ignored, just free parking until static is turned back on. At this time macaddr is just a placeholder for the unhandled issue of CCIO with no EEPROM. Finally, phyaddr will enable multiple card/IP/hostname on one CoCo soon.

iface eth0 inet static<br>
    address 10.2.2.123<br>
    gateway 10.2.2.1<br>
    netmask 255.255.255.0<br>
    macaddr 5C:26:0A:01:02:03<br>
    phyaddr $FF68<br>
<br>
iface eth1 inet static<br>
    address 192.168.1.7<br>
    gateway 192.168.1.1<br>
    netmask 255.255.255.0<br>
    macaddr 5C:26:0A:C0:C0:03<br>
    phyaddr $FF78<br>
<br>
 <b>/dd/SYS/hosts</b> stores basic network information. First we self identify, 127.xxx provides a hostname for each interface in list order. (Note f.dhcp only uses left 4 chars of a hostname).  Then a trusted list of IP/hostname pairs. These are used in place of dns calls. Below the comment lies our live dns cache. When a hostname must be looked up, the result is saved here and used until it stops working. Then it's erased, we look up the name again and add to the bottom.

   127.0.0.1   coco.conect.lan
   127.0.0.2   port2.conect.lan
   192.168.23.49 rickHP.conect.lan
   104.192.7.102 play-classics.net
   206.163.230.108 www.lcurtisboyle.com
   ; dns cache below
   76.13.32.141   www.aol.com
   142.250.1.106   www.google.com


/dd/SYS/resolv.conf normally lists up to 3 nameservers. If the first one fails to find a name the next ones have a go in turn.  'search' is a string added to any short hostname, so 'coco' becomes 'coco.conect.lan' for name searching purposes. We've overloaded this to store an extra dhcp provided nameserver if we are given one. The comment and nameserver pair will be removed or reset next network startup (any style, dhcp or static). 
One option is supported, ndots specs how many dots to avoid adding 'search'. Rotate and attempts are in there, but leaving next rev. 

   search conect.lan
   ; temporary, from dhcp 
   nameserver 192.168.1.254
   nameserver 8.8.8.8
   nameserver 1.1.1.1
   options ndots:1
