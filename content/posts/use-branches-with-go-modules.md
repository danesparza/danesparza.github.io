---
title: "Use Branches With Go Modules"
date: 2022-12-22T10:24:05-05:00
url: /2022/12/use-branches-with-go-modules
---

When developing with Go on a team, it is useful to have a good [branching strategy](https://www.atlassian.com/agile/software-development/branching) so you can work together as a team and not tromp on each others changes.

But how do you use [Go modules](https://go.dev/blog/using-go-modules) with a branching strategy? It's easy to refer to another project (even at a [certain version](https://go.dev/doc/modules/version-numbers)) -- but branches seem to offer a bit of a challenge.

The official guidance involves using the specific commit hash for the module you want, like this:

``` go
go get github.com/someone/some_module@af044c0995fe
```

Or hand-editing the `go.mod` file to include that specific commit hash, like this:

``` go
module /my/module

require (
...
github.com/someone/some_module v0.0.0-20181121201909-af044c0995fe
...
)
```

But finding that hash can be tricky sometimes. And what if you just want to make the substituion temporarily using the [replace syntax](https://go.dev/ref/mod#go-mod-file-replace) in `go.mod`?

Lucky for us, there is a simpler solution: Use the branch name when using the replace directive. It looks like this at the bottom of your `go.mod` file:

``` go
replace github.com/owner/repo-name => github.com/owner/repo-name branch-name
```

Notice the space between the repo and the branch name -- that's important.

If you try to do a `go install ./...` on the project, go will prompt you to do a `go mod tidy`.

When you do this, go _automatically updates the replace directive to use the appropriate semver version instead of the branch name!_

Now the go.mod line will look like this:

```
replace github.com/owner/repo-name => github.com/owner/repo-name v1.36.1-0.20221130164838-4a00d9d3a2fa
```

It's so handy!
