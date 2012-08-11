---
layout: post
title: Identifying your Ruby implementation at runtime
tags: 
---
There are a number of global variables available to determine, at runtime,
which Ruby implementation you are using. You can find most of these in
`Object.constants` and the most interesting ones are:

* RUBY_ENGINE,
* RUBY_PLATFORM,
* RUBY_VERSION and
* PLATFORM

Why would you want to know this? For instance to be able to load a Java library
when our code is running in JRuby, but fallback to some other library when we
are running in MRI or Rubinius. In my current use case: to run most specs in
MRI, which starts way faster than JRuby. Loading of Java-specific code is
surrounded by:

``` ruby
  if RUBY_ENGINE == jruby
    # Load jars and stuff
  else
    logger.warn "Running in stubbed mode: only useful for specs that
        dont exercise the actual database connection."
  end
```

