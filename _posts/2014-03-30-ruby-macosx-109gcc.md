---
layout: post
title: "jekyll 在MacOSX 10.9下的GCC编译参数问题"
description: ""
category: blog
tags: [ruby,mac]
---

## 问题复现 ##
* jekyll这么高大上的东西我竟然现在才知道，太受打击了。赶紧装起。

* 使用下面的命令安装先

	```
	YauzZMac-Air:~ YauzZ$ sudo gem install jekyll
	```

* 然后就是这样的结果
		
	```
	YauzZMac-Air:yauzz.github.io YauzZ$ sudo gem install jekyll
	Password:
	/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/ruby/2.0.0/universal-darwin13/rbconfig.rb:212: warning: Insecure world writable dir /usr/local/bin in PATH, mode 040777
	Building native extensions.  This could take a while...
	ERROR:  Error installing jekyll:
		ERROR: Failed to build gem native extension.
	
	/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby extconf.rb
	/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/ruby/2.0.0/universal-darwin13/rbconfig.rb:212: warning: Insecure world writable dir /usr/local/bin in PATH, mode 040777
	creating Makefile
	
	make "DESTDIR=" clean
	
	make "DESTDIR="
	compiling porter.c
	porter.c:359:27: warning: '&&' within '||' [-Wlogical-op-parentheses]
	      if (a > 1 || a == 1 && !cvc(z, z->k - 1)) z->k--;
	                ~~ ~~~~~~~^~~~~~~~~~~~~~~~~~~~
	porter.c:359:27: note: place parentheses around the '&&' expression to silence this warning
	      if (a > 1 || a == 1 && !cvc(z, z->k - 1)) z->k--;
	                          ^
	                   (                          )
	1 warning generated.
	compiling porter_wrap.c
	linking shared-object stemmer.bundle
	clang: error: unknown argument: '-multiply_definedsuppress' [-Wunused-command-line-argument-hard-error-in-future]
	clang: note: this will be a hard error (cannot be downgraded to a warning) in the future
	make: *** [stemmer.bundle] Error 1
	
	make failed, exit code 2
	
	Gem files will remain installed in /Library/Ruby/Gems/2.0.0/gems/fast-stemmer-1.0.2 for inspection.
	Results logged to /Library/Ruby/Gems/2.0.0/extensions/universal-darwin-13/2.0.0/fast-stemmer-1.0.2/gem_make.out
	```
	
## 怎么解决 ##

* 高大上的用下面这条命令去运行gem

	```
	YauzZMac-Air:yauzz.github.io YauzZ$ CONFIGURE_ARGS="--with-ldflags='-Wno-error=unused-command-line-argument-hard-error-in-future'" sudo -E gem install jekyll
	```

* 然后你就会得到这样的结果
	
	```
	YauzZMac-Air:yauzz.github.io YauzZ$ CONFIGURE_ARGS="--with-ldflags='-Wno-error=unused-command-line-argument-hard-error-in-future'" sudo -E gem install jekyll
	/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/ruby/2.0.0/universal-darwin13/rbconfig.rb:212: warning: Insecure world writable dir /usr/local/bin in PATH, mode 040777
	Building native extensions.  This could take a while...
	Successfully installed fast-stemmer-1.0.2
	Fetching: classifier-1.3.4.gem (100%)
	Successfully installed classifier-1.3.4
	Fetching: rb-fsevent-0.9.4.gem (100%)
	Successfully installed rb-fsevent-0.9.4
	Fetching: ffi-1.9.3.gem (100%)
	Building native extensions.  This could take a while...
	Successfully installed ffi-1.9.3
	Fetching: rb-inotify-0.9.3.gem (100%)
	Successfully installed rb-inotify-0.9.3
	Fetching: rb-kqueue-0.2.2.gem (100%)
	Successfully installed rb-kqueue-0.2.2
	Fetching: listen-1.3.1.gem (100%)
	Successfully installed listen-1.3.1
	Fetching: maruku-0.7.0.gem (100%)
	Successfully installed maruku-0.7.0
	Fetching: yajl-ruby-1.1.0.gem (100%)
	Building native extensions.  This could take a while...
	Successfully installed yajl-ruby-1.1.0
	Fetching: posix-spawn-0.3.8.gem (100%)
	Building native extensions.  This could take a while...
	Successfully installed posix-spawn-0.3.8
	Fetching: pygments.rb-0.5.4.gem (100%)
	Successfully installed pygments.rb-0.5.4
	Fetching: highline-1.6.21.gem (100%)
	Successfully installed highline-1.6.21
	Fetching: commander-4.1.6.gem (100%)
	Successfully installed commander-4.1.6
	Fetching: safe_yaml-1.0.1.gem (100%)
	Successfully installed safe_yaml-1.0.1
	Fetching: colorator-0.1.gem (100%)
	Successfully installed colorator-0.1
	Fetching: redcarpet-2.3.0.gem (100%)
	Building native extensions.  This could take a while...
	Successfully installed redcarpet-2.3.0
	Fetching: blankslate-2.1.2.4.gem (100%)
	Successfully installed blankslate-2.1.2.4
	Fetching: parslet-1.5.0.gem (100%)
	Successfully installed parslet-1.5.0
	Fetching: toml-0.1.1.gem (100%)
	Successfully installed toml-0.1.1
	Fetching: jekyll-1.5.1.gem (100%)
	Successfully installed jekyll-1.5.1
	Parsing documentation for blankslate-2.1.2.4
	Installing ri documentation for blankslate-2.1.2.4
	Parsing documentation for classifier-1.3.4
	Installing ri documentation for classifier-1.3.4
	Parsing documentation for colorator-0.1
	Installing ri documentation for colorator-0.1
	Parsing documentation for commander-4.1.6
	Installing ri documentation for commander-4.1.6
	Parsing documentation for fast-stemmer-1.0.2
	unable to convert "\xCF" from ASCII-8BIT to UTF-8 for ../../extensions/universal-darwin-13/2.0.0/fast-stemmer-1.0.2/stemmer.bundle, skipping
	unable to convert "\xCF" from ASCII-8BIT to UTF-8 for lib/stemmer.bundle, skipping
	Installing ri documentation for fast-stemmer-1.0.2
	Parsing documentation for ffi-1.9.3
	Installing ri documentation for ffi-1.9.3
	Parsing documentation for highline-1.6.21
	Installing ri documentation for highline-1.6.21
	Parsing documentation for jekyll-1.5.1
	Installing ri documentation for jekyll-1.5.1
	Parsing documentation for listen-1.3.1
	Installing ri documentation for listen-1.3.1
	Parsing documentation for maruku-0.7.0
	Installing ri documentation for maruku-0.7.0
	Parsing documentation for parslet-1.5.0
	Installing ri documentation for parslet-1.5.0
	Parsing documentation for posix-spawn-0.3.8
	unable to convert "\xCF" from ASCII-8BIT to UTF-8 for ../../extensions/universal-darwin-13/2.0.0/posix-spawn-0.3.8/posix_spawn_ext.bundle, skipping
	unable to convert "\xCF" from ASCII-8BIT to UTF-8 for lib/posix_spawn_ext.bundle, skipping
	Installing ri documentation for posix-spawn-0.3.8
	Parsing documentation for pygments.rb-0.5.4
	Installing ri documentation for pygments.rb-0.5.4
	Parsing documentation for rb-fsevent-0.9.4
	Installing ri documentation for rb-fsevent-0.9.4
	Parsing documentation for rb-inotify-0.9.3
	Installing ri documentation for rb-inotify-0.9.3
	Parsing documentation for rb-kqueue-0.2.2
	Installing ri documentation for rb-kqueue-0.2.2
	Parsing documentation for redcarpet-2.3.0
	unable to convert "\xCF" from ASCII-8BIT to UTF-8 for ../../extensions/universal-darwin-13/2.0.0/redcarpet-2.3.0/redcarpet.bundle, skipping
	unable to convert "\xCF" from ASCII-8BIT to UTF-8 for lib/redcarpet.bundle, skipping
	Installing ri documentation for redcarpet-2.3.0
	Parsing documentation for safe_yaml-1.0.1
	Installing ri documentation for safe_yaml-1.0.1
	Parsing documentation for toml-0.1.1
	Installing ri documentation for toml-0.1.1
	Parsing documentation for yajl-ruby-1.1.0
	unable to convert "\xCF" from ASCII-8BIT to UTF-8 for ../../extensions/universal-darwin-13/2.0.0/yajl-ruby-1.1.0/yajl/yajl.bundle, skipping
	unable to convert "\xCF" from ASCII-8BIT to UTF-8 for lib/yajl/yajl.bundle, skipping
	Installing ri documentation for yajl-ruby-1.1.0
	20 gems installed
	YauzZMac-Air:yauzz.github.io YauzZ$ jekyll -v
	/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/ruby/2.0.0/universal-darwin13/rbconfig.rb:212: warning: Insecure world writable dir /usr/local/bin in PATH, mode 040777
	jekyll 1.5.1
	```
* 安装成功

## 为什么呢 ##

* [参考这位老兄的blog](http://www.getchef.com/blog/2014/03/26/breaking-changes-xcode-5-1-osx/)

> The implementation of the GCC compatibility frontend in Xcode 5.1 (clang for LLVM) introduces a breaking change. In previous versions of Xcode, clang would accept unknown command line flags with a warning. As of Xcode 5.1, passing unknown flags causes rejection with an error. Clearly, rejecting unknown flags is sane behavior and Xcode is now arguably doing the right thing. This change highlights a bug that has been generating a mostly quiet failure for far too long.

* 原来是Xcode 5.1 把之前的一个警告升级为错误，造成了clang for LLVM的编译错误。

> A patch for this bug has been merged into upstream source and if you build Ruby from recent source on 1.9.3, you can bypass the issue entirely. However, there is not an official Ruby release containing this patch available yet.

* 最新的ruby已经Fix了这个bug，相信再不久以后这篇文章就会过时。




