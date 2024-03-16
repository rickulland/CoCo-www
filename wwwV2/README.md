www started as one Basic09 workspace. When that ran out, there was no way to split things up and not breed many, many bugs. Ask me how I know. ALong that line, the system utilties ifup, host and the like have moved into a parallel 'OS9utils' directory, along with all mention of the Frobio utility set.  

Anyway, we've defined a minimumal web browser as rendering text markup as well as the local display will allow, while including tradional navigation components (bookmarks, history, links), a dollop of file transfer (including image display) and finally simple forms. That's a lot, so here's WWW.V2, shedding load all over. The www process will end up being 100% UI (user interface) oriented to HTML in > menu > #path out. So we've put the OS9 menus _back_ in WWW to allow extra-menual affairs, for example mouse clicks outside the menu area may intersect an array of ‘hit boxes’ describing onscreen links. 

With these prereqs set up, you can run the code described below, all loaded into one B09 workspace. This dividing line will change as code is proven, packed, and stashed in a module. For now, I'd save all of these ( save* dome.b09 ) for one step loading.


<b>www</b>
This is strictly demo quality. WWW depends on being the only thing running, unless it is running all the other things. For debugging, the list below is included until they are truely debugged and turned into a module.  

<b>doBookmark</b>
Select from, or append/delete/edit any of bookmark.www, history.www, or link.www from inside the browser.

<b>gotoHost</b>
Some bare metal for now. Raw pokes to connect to a bookmark.server, if successful send a GET bookmark.page

<b>getdns</b>
An appendix, like, the thing that can get appendicits. gotoHost calls this to get an IP address for ‘hostname’. First checks dd/SYS/hosts, if nothing found shell to the host command and create one. Will be assimilated into host where it belongs.

<i>drawTable</i>
Next feature up. I couldn't find a way to emulate tables in a bookmark file, so we made a table file.  I see a RAMdisk in the future.

And after all this, you'll probably get only a few screens... The next bug is not pausing properly at end of page /html. The server gets mad and we find a ‘bad request’ header in the receive buffer. Coming soon!
