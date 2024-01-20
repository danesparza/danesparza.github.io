---
date: "2015-10-17T14:08:44-05:00"
title: "A modern Javascript workflow"
url: /2015/10/modern-javascript-workflow
---

Let's talk about your development workflow.  If you're still including all of your scripts in your pages using a `<script>` tag, you're doing it wrong.  A modern front-end workflow includes some kind of **dependency management** solution, and some kind of **bundling / minification** process. 

## Prereqs: Package management and Git console
### npm
Even if you don't use [Node.js](https://nodejs.org/) for your server **use [npm](https://docs.npmjs.com/getting-started/what-is-npm) for managing your client-side dependencies**.  It really makes adding and removing compenents a breeze!  

To get npm, just install Node.  Installation is a snap, and should only take a few minutes.  [Installers are available for all platforms](https://nodejs.org/).

### package.json
Also take a few minutes to [understand the package.json](http://browsenpm.org/package.json) file format.  It's basically an npm project file for your app -- it tells npm things about your app, your app dependencies, and your development time dependencies (like script bundlers and transpilers).  It's a pretty [widely adopted standard](https://github.com/search?q=package.json&type=Code&utf8=%E2%9C%93) now. 

### A command line with less suck
If you're doing development in Windows, the built-in console sucks.  Try installing [MsysGit for windows](https://git-for-windows.github.io/) and just opening a Git bash prompt on your development directory (by right-clicking on it and selecting 'Git Bash here').  Then you get tab-completion, history, and lovely colored output all for free!

## Dependency management
### Browserify
For dependency management, I prefer [Browserify](http://browserify.org/) and npm.  Browserify provides a Node flavored way to `require` javascript modules.  For example, including the excellent [MomentJS library](http://momentjs.com/) in your code is as simple as adding `var Moment = require('moment');` to your javascript source.

### uglify
For building/minification I prefer a simple [npm script](https://github.com/danesparza/Dashboard/blob/master/package.json#L48) and the unfortunately named [uglify](https://github.com/mishoo/UglifyJS2).  This, combined with the `package.json` file provide a very nice way to install all the script dependencies your app requires.  

### Babel
With ES6 (the next version of Javascript) right around the corner, why not start using those new features now?  Yes.  Even if your current browser doesn't support them.  

How?  Using a **transpiler**, of course!  

I prefer using [Babel](https://babeljs.io/) as a transpiler (which is also quickly becoming a standard).

[Learn more about ES6 (also called ES2015) here](https://babeljs.io/docs/learn-es2015/).  In the meantime ... 

## Installing it all
To install Browserify, uglify and Babel using npm, drop to a command line and run:

	npm install -g browserify uglify-js babel

## Tying it all together
A package.json from [my most recent single-page-application](https://github.com/danesparza/Dashboard) looks like this:

	{
	  "name": "family-dashboard",
	  "description": "Family dashboard",
	  "version": "0.1.3",
	  "author": "Dan Esparza",
	  "browserify": {
	    "transform": [
	      "babelify",
	      "envify"
	    ]
	  },
	  "bugs": {
	    "url": "https://github.com/danesparza/Dashboard/issues"
	  },
	  "dependencies": {
	    "cookies-js": "^1.0.0",
	    "director": "^1.2.7",
	    "flux": "^2.1.1",
	    "keymirror": "~0.1.0",
	    "moment": "^2.9.0",
	    "moment-timezone": "^0.3.0",
	    "object-assign": "^1.0.0",
	    "react": "^0.13.3",
	    "react-radio-group": "2.1.1"
	  },
	  "devDependencies": {
	    "babelify": "^6.3.0",
	    "browserify": "^6.2.0",
	    "envify": "^3.0.0",
	    "uglify-js": "~2.4.15",
	    "watchify": "^2.1.1"
	  },
	  "keywords": [
	    "calendar",
	    "dashboard",
	    "flux",
	    "google",
	    "react",
	    "weather"
	  ],
	  "license": "MIT",
	  "main": "js/app.js",
	  "repository": {
	    "type": "git",
	    "url": "https://github.com/danesparza/Dashboard.git"
	  },
	  "scripts": {
	    "build": "browserify js/app.js -t babelify | uglifyjs -cm > js/bundle.js",
	    "start": "watchify -v -t babelify js/app.js -o js/bundle.js"
	  }
	}

Pay close attention to the `scripts` section, above.  

My finished workflow looks like this...  

When I want to install a new application dependency, add it to the `package.json` file and run:

	npm install

When I want to develop some code, I drop to a git bash prompt and run:
	
	npm start

This will automatically watch for changes and rebuild my bundle.js whenever a change is made to any javascript file in the app.

When I want to do a full minimized release build, I just run:

	npm run build


### Next up...
Next, I'll be checking out [webpack](https://webpack.github.io/) to replace Browserify for even more fancy features!