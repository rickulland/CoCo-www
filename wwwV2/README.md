www started as one Basic09 workspace. When that ran out, there was no way to split things up and not breed many, many bugs. Ask me how I know. 

Anyway, we've defined a minimumal web browser as rendering text markup as well as the local display will allow, while including tradional navigation components (bookmarks, history, links), a dollop of file transfer (including image display) and finally simple forms. That's a lot, so here's WWW.V2, shedding load all over. The www process will end up being 100% UI (user interface) oriented to HTML in > menu > #path out. So we've put the OS9 menus _back_ in WWW to allow extra-menual affairs, for example mouse clicks outside the menu area may intersect an array of ‘hit boxes’ describing onscreen links. 

 To get www running, the first step is install frobio https://github.com/strickyak/frobio/.  Check out the 'built' directory! Copy to /dd/CMDS/f.dig, etc and use 'attr filename e pe' to make them all runnable. Second, load and pack 'host.b09' and 'ifup.b09' as separate programs named host and ifup, also put them in /dd/CMDS. And finally, edit the example interfaces and hosts files, put them in /dd/SYS/... 
 
 At this point load 'testHost' in B09, feed it hostnames and get IP addresses. Usually, it's simple minded yet. Occasional success means these prereqs are working:

<b>host name.domain.tld</b>
The start of a local dns cache, host.b09 is recursive, it first checks /dd/SYS/tempdns.txt for preexisting work. If the earlier result includes a CNAME, but includes no A record, host does another f.dig and restarts. For now, we assume eventually there will be an A record, parser work needed (SOA, etc will hang). But provided we eventually found an A record, the IP and original hostname (not the CNAMEs) are appended to /dd/SYS/hosts and we return the IP address to the caller. 

If there is no /dd/SYS/tempdns.txt, the first new work is searching /dd/SYS/hosts. No match, f.dig and rerun. In other words, once cached the second search is free. To illustrate, search for www.aol.com. Now delete it from /etc/hosts and do it again. The entry will return after a time:-). In host.b09, set debug:=TRUE to watch the lookups roll by (3 in this case). 

<b>ifconfig</b>
Not really, but if the chip is configured it prints the basic IPv4 config data it's configured with. If done while nothing else is happening, that is.

<b>ifup</b>
Uses /dd/SYS/interfaces, only 1 interface for now. It supports the Linux trick of setting a full static interface in the file, then change the one word 'static' to 'dhcp' when traveling.  


With these prereqs set up, you can run the code described below, all loaded into one B09 workspace. This dividing line will change as code is proven, packed, and stashed in a module to be inserted above. 


<b>www</b>
This is strictly demo quality. WWW depends on being the only thing running, unless it is running all the other things. For debugging, the list below is included until they are truely debugged and turned into a module.  

<b>doBookmark</b>
Select from, or append/delete/edit any of bookmark.www, history.www, or link.www from inside the browser.

<b>gotoHost</b>
Some bare metal for now. Raw pokes to connect to a bookmark.server, if successful send a GET bookmark.page

<b>getdns</b>
An appendix, like, the thing that can get appendicits. gotoHost calls this to get an IP address for ‘hostname’. First checks dd/SYS/hosts, if nothing found shell to the host command and create one. Will be assimilated.

<i>drawTable</i>
RSN. I couldn't find a way to emulate tables in a bookmark file, so me made a different file. 


And after all this, you'll probably get only a page or two. The next bug is not pausing properly at end of page /html. The server gets mad and we find a ‘bad request’ header in the receive buffer. Been ignoring this until now;-)



