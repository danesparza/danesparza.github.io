---
date: "2012-04-04"
url: /2012/04/using-ms-onenote-to-manage-bug-and-release-information
title: "Using MS Onenote to manage bug and release information"
---
### The problem
Interruptions <a href="http://www.codinghorror.com/blog/2006/09/the-multi-tasking-myth.html">cause a real problem with lost time</a> during software development. <a href="http://www.codinghorror.com/blog/">Jeff Atwood</a> summarizes this nicely:
<blockquote>Even adding a single project to your workload is profoundly debilitating by Weinberg's calculation. <strong>You lose 20% of your time</strong>. By the time you add a third project to the mix, nearly half your time is wasted in task switching.</blockquote>
As a software developer, I'm constantly trying to avoid getting distracted by production incidents, other bugs, and high priority features that need to get implemented. Don't get me wrong -- I think each of those things is a necessary (evil) part of software development.

### The solution: taking notes
We're already using the computer to program, I figure it makes more sense to take notes on the computer (vs pen and paper). After trying to use Evernote and getting frustrated with the lack of a decent print view or formatting options, I chose <a href="http://office.microsoft.com/en-us/onenote/">MS Onenote</a> and haven't looked back.

<!--more-->
### Why Onenote?
<ul>
	<li>My data is <strong>easy to organize</strong>... need to create to rearrange your notebook pages? Just drag and drop. Need to type notes? Just type anywhere. Need to indent certain pages under others? Just drag to indent.</li>
	<li>My data is stored in the filesystem and backed up and <strong> sync'd via <a href="http://www.dropbox.com/">Dropbox</a></strong>. (Optionally, your data can get sync'd via Skydrive, but I've had bad experiences going this route).</li>
	<li><strong>Quickly format text</strong> in tables, lists, headings and highlighting</li>
	<li>Sometimes ideas are hard to express with words - Easy to use <strong>drawing capabilities</strong> are built in. Several times I've used Onenote as a virtual whiteboard to share visual ideas with co-workers when screen sharing.</li>
	<li>It's <strong>easy to spit out a <em>nicely</em> formatted PDF</strong>. Something that looks professional with no effort on my part. Something that I can show my boss. This can't be overstated.</li>
</ul>

### Using a template to ease your workflow
Let's face it: Note taking is <em>tedious</em>. Somewhere after achieving amateur note taker status, you'll either want to give up or make life much easier for yourself somehow -- because taking notes on a bug every time will begin to feel very repetitive or even worse: a waste of time.

<em><strong>Templates</strong> make your life easier in MS Onenote.</em>

Before creating a template, think about:
<ul>
	<li>What kinds of items do you find yourself wanting to capture over and over again?</li>
	<li>If you were a developer digging through this bug again in 3 months, what helpful information would you want laid out in front of you?</li>
</ul>
Next, create a note with the appropriate content to prompt you, then <a href="http://office.microsoft.com/en-us/onenote-help/create-a-template-in-microsoft-office-onenote-2010-HA101998216.aspx">save as a template</a>. Finally, use the down arrow by 'New Page' to select your new template. It's just that simple.

<img class="alignnone size-full wp-image-313" title="How to pick a template" src="/img/2577.on_blog_template_ui.png" alt="" width="342" height="232" />

### Sample template content
My default bug fixing template has the following information in it (or if you already have Onenote, just <a href="/downloads/Bug_worksheet.zip">download and unzip this sample</a>):
<ul>
	<li><strong>Default text in the title</strong> to help format it correctly</li>
	<li><strong>An overview</strong>, with the heading 'Overview' and some text to prompt me, like this: <em>"Summarize the issue in plain English. Indicate any connections to other bugs here, and if you're planning on getting help from specific developers (and what they're going to help with)."</em></li>
	<li><strong>A 'related incidents' section</strong> with a table that includes 2 columns: Incident # and Description</li>
	<li><strong>Setup notes</strong>, with an initial list item that has the following prompt: <em>List anything required to setup the project (IIS setup requirements, .dll references/includes, config changes, environment requirements, etc)</em></li>
	<li>A <strong>repro notes</strong> section to allow me to include complete steps to reproduce the problem.</li>
	<li>A <strong>source review</strong> section with a table that has the following columns: <strong>Item</strong> and <strong>Found here</strong>. This will allow me to specify where certain bits of code are located based on what they do. Your future co-workers will appreciate this.</li>
	<li>A <strong>source annotation review</strong> section. This is where I visit the <a href="http://msdn.microsoft.com/en-us/library/bb385979.aspx">source annotation</a> (<a href="http://book.git-scm.com/5_finding_issues_-_git_blame.html">sometimes</a> <a href="http://tortoisesvn.tigris.org/blame.html">called</a> '<a href="http://stackoverflow.com/a/2228395/19020">blame</a>') view of the repository to try and determine who last touched the lines of code I'm looking at -- and what led them to make those changes. Hopefully they have check-in comments that indicate why they changed the source. The point of this section is not to lay blame but to leave a trail of crumbs. If something is confusing, ask these developers first.</li>
	<li>A <strong>debugging notes</strong> section. Perhaps one of the most important sections. If you have bugs that take several days to work through, the notes you take here will be invaluable. You'll find that as long as you take reasonable notes, you can set a problem down, enjoy lunch, and pick it back up again with ease -- and you'll know right where you left off.</li>
	<li>A <strong>new configuration items</strong> section. This includes a table with new configuration items and their descriptions.</li>
	<li>Finally, I also include <strong>a resolution timeline</strong> section. This is a table that includes the following rows: Smoke test successful, Code review, and checked-in. Each of these items has a date/time that I fill in when it's complete. It serves as a record of when certain items are completed, and can be helpful when putting together a status report.</li>
</ul>

### Tips
<ul>
	<li><strong>Create a visual hierarchy based on releases</strong>. Also: Create a release information template to use as the top-most tab for a release. It should include important dates for a given release.</li>
	<li>Save to PDF and <strong>include your notes in emails and bug reporting software</strong>. You'll be the envy of your co-workers. (I've actually had QA specifically request the bugs I fix because the notes I take make their life easier)</li>
</ul>
