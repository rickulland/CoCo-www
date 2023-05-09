The start of a new browser. Original www was written as one Basic09 workspace. When it ran out of said space, there was no way to split things up and not breed many, many bugs. Ask me how I know.  
V2 begins anew with these basic utilities. You will need the frobio commands copied to /dd/SYS/CMDS/f.dig, etc. And then use 'attr filename e pe' to make them all runnable. Edit the example interfaces and hosts, put them in /dd/SYS/...

Biggest thing first, the start to a local dns cache

<b>host name.domain.tld</b>
Host.b09 is recursive, it first checks /dd/SYS/tempdns.txt for preexisting work. If the earlier result includes a CNAME, but includes no A record, host does another f.dig and restarts. For now, we assume eventually there will be an A record, parser work needed (SOA, etc will hang). But provided we eventually found an A record, the IP and original hostname (not the CNAMEs) are appended to /dd/SYS/hosts and we return the IP address to the caller. 

If there is no /dd/SYS/tempdns.txt, the first new work is searching /dd/SYS/hosts. No match, f.dig and rerun. In other words, the second search is free. To illustrate, search for www.aol.com. Now delete it from /etc/hosts and do it again. The entry will return after a time:-). In host.b09, set debug:=TRUE to watch the lookups roll by (3 in this case). 

<b>testit</b>
The 'host' program isn't meant to run standalone. Run this test frame to feed it a hostname and get an IP. Or an eternal loop. It will (usually) update /dd/SYS/hosts, build a dns cache from here.

<b>ifconfig</b>
Not really, but if the chip is configured it prints the basic IPv4 config data it's configured with.

<b>ifup</b>
Uses /dd/SYS/interfaces, only 1 interface for now. It supports the Linux trick of setting a full static interface in the file, then change the one word 'static' to 'dhcp' when traveling.  

Notes: I miss sed.

<b>editbookmark</b>
Manage bookmark.www, history.www, or even link.www if you want to.

<b>filefix</b>
If you've got an established bookmark file from the original www, use this to convert to the new bookmark.www.