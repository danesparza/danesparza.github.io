---
date: "2014-09-24T09:42:44-04:00"
title: "Use Keepass and dropbox to manage your passwords"
url: /2014/09/using-keepass-and-dropbox-to-manage-passwords
---

Interested in using a password manager for all your passwords?  

You should be.  

Services like [OpenId](http://openid.net/get-an-openid/what-is-openid/) are catching on, but aren't as widespread as originally hoped.  You might visit hundreds of different sites a year that use a login and password.  

Using the same password for every website and service is a terrible idea:  A hacker just needs to [compromise one](http://www.nbcnews.com/id/20534586/ns/technology_and_science-security/t/monster-security-breach-teaches-lessons/#.T5CVJ6VYu0A) of the [services](http://www.nytimes.com/2014/09/05/us/hackers-breach-security-of-healthcaregov.html?_r=0) you visit and they've got the username/password for [all the sites you visit](https://www.privacyrights.org/data-breach/new).  Ideally, they should all have different passwords -- but how in the world could you remember all of them? 

### Keepass to the rescue

This is where [Keepass](http://keepass.info) comes in.  Keepass is a free, open source password manager for Windows (with OSX and iOS equivalents):

<img class="img-responsive" src="/img/Keepass_main.png" alt="Keepass main screen" />

Open source is important when it comes to security -- if you're a developer, you can actually make sure the algorithms used to secure your data are being used properly.  Keepass stores your passwords in a secure file-based database.

### Generate a new username and password for every site

Generating a new login for a site is really simple with Keepass.  First, make sure that you've installed it from the KeePass website, here:  [http://keepass.info](http://keepass.info)

Create a [new database](http://keepass.info/help/base/firststeps.html).  You will pick a single password to 'unlock' Keepass and get to your other passwords.  Make sure it's different and not obvious.  It will be one of the last passwords you'll ever have to memorize.

1. With your database created and unlocked, select 'Edit / Add entry' - or right click and select 'Add entry'
2. Fill in the name of the site and service and the username you'd like to use.  Your password will be automatically generated.
3. Optional: Press the 'generate new password' button and create a password according to custom rules
4. Optional: Add a url for the site to make it easier to remember where to go to enter your credentials
5. Press 'OK' to create the new entry

It's just that simple!

### Backup and sync your passwords

Now that you have Keepass up and running you might ask, "But what if something terrible happens to my computer -- shouldn't I be backing this up?  What if I have multiple computers -- how do I securely share my passwords among all my machines?"

Not to worry -- this is where [Dropbox](https://db.tt/SJNHb94y) comes in.  Dropbox is a free service for up to 2GB of data (you can earn more with referrals, or just sign up for their monthly service -- it's not terribly expensive).

After [signing up for Dropbox](https://db.tt/SJNHb94y) and [getting it installed locally](https://www.dropbox.com/help/243), create a 'Keepass' directory under your dropbox.  Move (or create) your Keepass .kdbx file(s) to this directory.  When you're done, this should be the only location on your computer that has Keepass files.  

### Finishing up

When you start Keepass, make sure it opens the databases in the directories under Dropbox.  Make sure that Keepass is set to start automatically.  

If you have additional questions about Keepass, check out the [FAQ on the website](http://keepass.info/help/kb/faq.html).

If you have additional questions about Dropbox, check out the [Dropbox help center](https://www.dropbox.com/help).