# Learn Bower in x minutes

[TOC]

## What's Bower

Web sites are made of lots of things — frameworks, libraries, assets, utilities, and rainbows. Bower manages all these things for you.

Bower works by fetching and installing packages from all over. Bower keeps track of these packages in a manifest file, bower.json.

## Requisite

* [Node and npm](http://nodejs.org/)
* [Git](http://git-scm.org/)

## Install Bower

```sh
$ npm install -g bower
```

## Install Packages

Install packages with the following command:

```sh
$ bower install <package>
```

Bower installs packages to `bower_components/`

A package can be a GitHub shorthand, a Git endpoint, a URL, and more.

* Package name
* GitHub shorthand: user/repo
* Git endpoint: git://github.com/user/package.git
* URL: http://example.com/script.js

## Update Packages

Update packages with the following command:

```sh
$ bower update
```

## Search Packages

Please navigate to [Search Bower packages](http://bower.io/search)

## Save Packages

Save packages to bower.json with the following command:

```sh
$ bower init
```

Or

```sh
$ bower install --save <package>
```

`--save` adds the package as a dependency in bower.json.

## Reference

1. [Bower Website](http://bower.io/)


