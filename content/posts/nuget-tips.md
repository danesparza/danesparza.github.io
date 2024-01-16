---
date: "2015-04-03T14:42:44-04:00"
title: "NuGet tips"
url: /2015/04/nuget-tips
---

Created in 2010, [NuGet](http://docs.nuget.org/Consume/Overview) is a free and open source package manager for the Microsoft development platform. NuGet has become the defacto way of distributing tools and libraries in the Microsoft developer community.

If you're interested in working with NuGet, here are some tips and tricks that will be helpful to you. 

### Creating NuGet packages
Creating NuGet packages from scratch can be daunting for a first-timer.  

#### nuspec != nupkg
First, you need to understand that there is a difference between the types of files that NuGet uses:  

* A `.nupkg` file is the package that is installed on a destination machine. 
* A `.nuspec` is a template that is used to create versioned package files.

You can always refer to the [Nuspec reference](http://docs.nuget.org/create/nuspec-reference), but using the [NuGet package explorer](http://docs.nuget.org/Create/using-a-gui-to-build-packages) makes it a bit easier:  

<img class="img-responsive" src="/img/package-explorer-lib-folder.png" alt="Package exlorer screen" />

You can easily set your package name, version, author, and even the files in your NuGet package from the NuGet package explorer.  When you're done, remember to save the .nuspec file by selecting *'File / Save Metadata as...'*

#### Use the same name as your project
Wondering where to store your .nuspec file?  Store it in the same directory as your .csproj file and name it the same thing as your project.  

Example:

* Project name: `MailChimp.csproj`
* Nuspec name: `MailChimp.nuspec`

This gives you the option of using the version of nuget pack that allows you to [build your .nupkg directly from your project file](https://docs.nuget.org/Create/Creating-and-Publishing-a-Package):  `nuget pack foo.csproj`  

When you call it this way, NuGet will examine your project references and automatically include everything it needs to in the package.

#### Don't hardcode version numbers
Don't store the version number in your .nuspec file.  

Instead, use the `$version$` placeholder instead - this takes whatever version number you have in your AssemblyInfo.cs file.  

Alternatively, you can also pass in the version number on the command line when you generate your nuget package using the -Version parameter.  Example: `nuget pack foo.nuspec -Version 2.1.0`

There are many other [replacement tokens](https://docs.nuget.org/Create/NuSpec-Reference) you can use to automatically sync information in your NuGet package as well.

### Publishing packages on NuGet.org

Many NuGet packages are available on NuGet.org.  NuGet.org is the default package source for the NuGet plugin for Visual Studio.  If you decide to distribute your package on NuGet.org -- you'll instantly make it available to hundreds of thousands of developers around the world.

When creating a NuGet package for an open source project, start by creating an account [https://www.nuget.org](https://www.nuget.org) if you haven't already.

#### Where do I get my API key?
Your API key can be found on your NuGet.org account page (near the bottom) -- you'll need this when publishing nuget packages using various tools:  

* You can publish directly using the NuGet package explorer.  In the app, go to *File / Publish* and enter your API key where it prompts for 'Publish key'
* You can publish directly using using NuGet.  Using the [nuget command line tool](https://docs.nuget.org/consume/command-line-reference), you can supply your API key to publish: `nuget push <package path> [API key] [options]`

### Publishing packages inside your company
To publish your packages inside your company you'll need to first setup an internal NuGet server.  The easiest way to get up and running is to use the [NuGet.Server package](https://docs.nuget.org/create/hosting-your-own-nuget-feeds) to create, build, and deploy a mini NuGet server internally.  

Pushing NuGet packages is as simple as copying new packages to the source directory or using the `nuget push` command-line syntax.  I recommend you use one of these methods as part of a continuous integration build process.  

Next, your consuming dev teams just need to [add a new NuGet package source](https://docs.nuget.org/create/hosting-your-own-nuget-feeds) in their Visual Studio Package Manager.

Finding, installing and managing your internal packages is now just as easy as using NuGet!

#### Additional reading

* [Top 10 NuGet antipatterns](https://msdn.microsoft.com/en-us/magazine/jj851071.aspx)
* [LosTechies tips on NuGet packages](https://lostechies.com/joshuaflanagan/2011/06/23/tips-for-building-nuget-packages/)
* [Automatically build open-source NuGet packages with AppVeyor](http://www.appveyor.com/docs/deployment/nuget)