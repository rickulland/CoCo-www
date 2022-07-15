Here is a demo based on <a href="https://youtu.be/FJm2G_n1Gx0"> L Curtis Boyle and Canadian RetroThings video </a> presented at the last CoCoFest, plus additional work Curtis has done since then to enable hotkeys. If you have not seen it, do that first!    

So what is this project for? I've extracted it from an app that has many bits which do a thing, then pause for input. One menu to rule them all. If you need interactive menus, this exact structure won't work. For example a word processor would bake the menus into the text entry procedure. 

To use, ask the user if they have a mouse. Provide up to a screenful of output, then call doMenu. If hasmouse=FALSE, doMenu will print an inverted text menu line or two at the bottom of the filled screen, ie: "&lt;B&gt;ookmark   &lt;G&gt;oto page"... and accept single letter keyboard input until user matches a hotkey. If hasmouse=TRUE, user gets WindInt menus and scroll bars. Soon <i>(thanks Curtis!)</i> with hotkeys. So click open+click item, or in EOU 1.0, avoid that with ALT+hotkey.

Your calling program will get back a call number, and could do something like:

	RUN doMenu(hasmouse,menucall)
	ON menucall GOSUB 10,20,30...

And thats it!


Of course it isn't. You have to write menus. Here is a chart of menu codes for an unspecified program. Menu_ids < 32 are predefined for everyone, so this first block of  WindInt scroll bars and quit button can be copypaste right over.
<table>
<tr><td>id</td><td>item</td><td>hotkey</td><td>call</td><td>label</td></tr>
<tr></tr>
<tr><td>2</td><td></td><td>q</td><td>1</td><td>quit</td></tr>
<tr><td>4</td><td></td><td></td><td>2</td><td>scroll up</td></tr>
<tr><td>5</td><td></td><td>$13</td><td>3</td><td>scroll down</td></tr>
<tr><td>6</td><td></td><td></td><td>4</td><td>scroll right</td></tr>
<tr><td>7</td><td></td><td></td><td>5</td><td>scroll left</td></tr>
</table>

Menus above 32 are definable. And so we will define menu 33 as 'Bookmark' and menu 34 as 'Page', listing our possible choices 1-? under each, and finally assign unique hotkey and 'call' number across the lot, to be passed around between procedures.

<table>
<tr><td>id</td><td>item</td><td>hotkey</td><td>call</td><td>label</td></tr>
<tr><td>33</td><td></td><td></td></td><td></td><td>Bookmark</td></tr>
<tr><td>1</td><td></td><td>b</td><td>6</td><td>&lt;B&gt;M this</td></tr>
<tr><td>2</td><td></td><td>g</td><td>7</td><td>&lt;G&gt;oto BM</td></tr>
<tr><td>3</td><td></td><td>m</td><td>8</td><td>&lt;M&gt;anage BM</td></tr>
<tr><td>4</td><td></td><td>l</td><td>9</td><td>&lt;L&gt;inks</td></tr>
<tr><td>34</td><td></td><td></td></td><td></td><td>Page</td></tr>
<tr><td>1</td><td></td><td>c</td><td>10</td><td>&lt;C&gt;lear</td></tr>
<tr><td>2</td><td></td><td>r</td><td>11</td><td>&lt;R&gt;eload</td></tr>
<tr><td>3</td><td></td><td>n</td><td>12</td><td>&lt;N&gt;ew tab</td></tr>
<tr><td>4</td><td></td><td>p</td><td>13</td><td>&lt;P&gt;rint</td></tr>
<tr><td>5</td><td></td><td>s</td><td>14</td><td>&lt;S&gt;ave</td></tr>
</table>

Program flow

Keyboard might mean low-end machine, we'll check first and run a little standalone keyboard menu lines 10-50, in a locked loop to force a valid respose. With our table, the edits are pretty obvious. 

Mouse begins translating the chart to gfx2 calls around lines 60-80, then entering the double loop that will again trap our users until they play nice. In the loop, we check for a keypress in the ALT-letter range, redshift to printable and get it's item # from the hotkey strings you have thoughtfully edited into these lines around 103,108:

	menu_item:=SUBSTR(hotkey,"bgml")
	menu_item:=SUBSTR(hotkey,"crnps")

If no hotkeys were pressed, we analyse the click with our user defined pachinko tree. Failing that, a final test for system menu clicks. ENDLOOP.
