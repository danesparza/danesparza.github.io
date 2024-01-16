---
date: "2011-06-10"
url: /2011/06/a-re-introduction-to-the-chrome-dev-tools
title: "A re-introduction to the Chrome dev tools"
---
Here is a great presentation on the Chrome developer tools by <a href="http://paulirish.com/">Paul Irish</a> and <a href="http://twitter.com/#!/repenaxa">Pavel Feldman</a>.  PDF notes <a href="http://static.googleusercontent.com/external_content/untrusted_dlcp/www.google.com/en/us/events/io/2011/static/notesfiles/ChromeDevToolsReloaded.pdf">available from Google</a>.  A delightful Chrome Developer tools cheatsheet is also available in <a href="https://github.com/borismus/DevTools-Lab/raw/master/cheatsheet/chromedev-cheatsheet.pdf">PDF</a> and <a href="https://github.com/borismus/DevTools-Lab/raw/master/cheatsheet/chromedev-cheatsheet.png">PNG</a> formats from Paul's colleague <a href="http://smus.com/">Boris Smus</a>.
<!--more-->
<iframe width="560" height="349" src="http://www.youtube.com/embed/N8SS-rUEZPg?rel=0" frameborder="0" allowfullscreen></iframe>


### General info
- Chrome developer tools are roughly equivalent to <a href="http://getfirebug.com/">Firebug</a> in <a href="http://www.mozilla.com/en-US/firefox/new/">Firefox</a> (but with some newly added cool features).
- Dev tools are actually made out of CSS / Javascript / etc themselves!

### Styling control and CSS manipulation
- Chrome keeps track of in-browser changes to the stylesheet (even has version control).  
- In the <a href="http://www.chromium.org/getting-involved/dev-channel">developer version</a> of the browser you can easily save your new stylesheet by right clicking on the stylesheet in 'resources' in Chrome and selecting 'Save as'
- You have full control of formatting in the style editor now too -- works closer to a full editor in the browser.

### Network interaction
- In Chrome, you can get raw request/response/cookies information sent for each request in a page
- You can also see an individual item's timing information (dns lookup / wait / transfer / etc)
- You see a <a href="http://code.google.com/chrome/devtools/docs/network.html">consolidated timeline</a>
- All of this information comes from the Chrome network stack

### Console API
- There is more than just console.log  (and you can pass lots of parameters to console.log, too)
- dir(node) gives you the DOM properties of the passed node
- inspect(node) jumps to the element pane, with the passed element selected
- $0 (in the console) refers to whatever element is currently selected in the 'element' pane - this is useful to execute script against the selected element
- The copy(text) method can copy text to the clipboard for you.  You can actually copy($0.innerHTML) to copy the selected element's body text to the clipboard.

### Script debugging
- You can use the <a href="http://code.google.com/chrome/devtools/docs/scripts-breakpoints.html">debugger with breakpoints</a> (just like you might use Visual Studio or Firebug)
- You can also reveal an element in the 'elements' pane, right click on the element in the pane and select 'break on subtree modifications'.  (Holy crap this is useful for debugging things you're unfamiliar with!)  Be sure to use the call stack information in the developer tools with this one.  The first item you're likely to see when using jQuery is some jQuery innards -- but go further up the call stack and you're likely to see what code is causing that bit of HTML to get modified.  <em><strong>Also:</strong>  check the user friendly message in the scripts panel that indicates why execution was paused -- it'll tell you exactly what element was modified.</em>
- You can also break on various DOM events (see the 'Event Listener Breakpoints' panel in 'scripts').  Useful for debugging timers & keyboard event bugs -- when you only know the type of event that might be triggering something.
- You can also explore page script innards using <a href="http://code.google.com/chrome/devtools/docs/timeline.html">the 'timeline' pane</a> - you can click on items in the left side of the timeline page and go to the line of code that triggered that part of the timeline in the scripts page.
- Just like the style changes, you can make script changes directly in the scripts panel almost like a nice script IDE.
- You can get nicely formatted code by pressing a tiny button in the bottom of the scripts panel (developer build only).  You can still use breakpoints after the code is reformatted.

