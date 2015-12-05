---
title: Pyston in Fedora and the Future of Python 2
layout: post
---

# Pyston?
[Pyston](https://github.com/dropbox/pyston) is a performance-focused Python interpreted using LLVM and JIT developed by couple people at Dropbox. It sounds a lot like PyPy, but unlike PyPy, Pyston keeps the C ABI/API compatibility, which should make it a truly drop-in replacement for CPython.

Eventually, anyway.

Currently at version 0.4 it's missing a [bunch of things](https://github.com/dropbox/pyston/blob/14aa0f0adadf944a8819ad081626d43fbe1cbb8c/test/CPYTHON_TEST_NOTES.md), but [reportedly it's capable of running a lot of Dropbox's servers](https://www.youtube.com/watch?v=NdB9XoBg5zI), so it can't be too bad.

# Future of Python 2?
Dropbox has said they won't be porting all their code to Python 3, so presumably Pyston is one of their fallbacks come 2020 -- [the year Python 2 ceases to be supported by upstream](https://www.python.org/dev/peps/pep-0373/).

Dropbox isn't alone either -- Google, OpenStack, and others have giant codebases not compatible with Python 3 and I imagine they aren't thrilled about the prospect of porting it all.

So what will it be: PyPy? Pyston? A third implementation? Or will CPython 2 just continue living beyond 2020, happily ever after?

# Pyston in Fedora
But back to Fedora! If you're not familiar with COPR, you can install Pyston using the commands below. Although the package does not include pip, Pyston is configured to pick up modules you install with Python 2 pip.

{% highlight bash %}
$ dnf install -y dnf-plugins-core
$ dnf copr enable -y mstuchli/pyston
$ dnf install -y pyston
$ pyston
{% endhighlight %}
