---
title: Python 3 Porting Database
layout: post
---

[![img](http://i.imgur.com/V17bYKv.png)](http://portingdb-encukou.rhcloud.com/)

Python is a very important building block of the Fedora experience. With the release of Fedora 23 we will have completed a step in the transition to Python 3: [Fedora 23 Workstation does not contain Python 2 in it's default installation at all](http://rkuska.github.io/2015/10/08/Fedora-and-Python3.html).

Next up, we want to make sure that all (or as many as possible) Python packages in Fedora support Python 3, or have a successor that does. We're still far from this being the case: of the about 3000 Python packages in Fedora, just 900 do support Python 3 or have an alternative that does. For this reason, [@encukou](https://twitter.com/encukou) has created [Python 3 Porting Database](http://portingdb-encukou.rhcloud.com/) ([github repo](https://github.com/fedora-python/portingdb/)), a tool that tracks progress in this goal and helps coordinate the efforts.

In this post I'd like to explain some of the features, show how the Database works and how you can contribute.

# Features

The UI should be fairly self-explanatory: The top contains a graph of all the Python packages in Fedora and their state. Below are state graphs for several package groups we're tracking (Fedora Infrastructure, OpenStack and so on).

This is followed by a list of the actual packages, sorted by their state. Clicking on a package name will bring you to it's subpage, which is designed to help you figure out if it's ready to be worked on, and if not, what needs to be done.

## Contributing

### Updating the Data

The simplest way of contributing to the Database is sending a pull request with updated yaml files (described below): If for instance you know that project FooGizmo supports Python 3 in upstream and BarTool's Python 3 subpackage in Fedora is being worked on and the Database says otherwise, you can change the upstream and fedora-update yamls as such:

### upstream.yaml
{% highlight dpatch %}
FooGizmo:
-     status: in-progress
+     status: released
{% endhighlight %}

### fedora-update.yaml
{% highlight dpatch %}
BarTool:
-    status: idle
+    status: in-progress
+    links:
+       bugzilla: https://bugzilla.redhat.com/show_bug.cgi?id=123456
{% endhighlight %}

And submit a pull request. Or just open an issue describing what needs to be done and we'll look into it ourselves. :)

### Porting!

The most difficult, but also the most valuable way of contribution is doing the actual porting. In that case you'll want to check the Database if all the dependencies are ported, make sure your package isn't being worked on by someone already and then update porters.yaml to let the world know you're porting. :)

That said we appreciate all manner of contributing, from letting us know some of the data is wrong, to porting a module of your choosing by yourself.

# How it works

Please keep in mind the following can change at any point.

## The fedora.json

The primary data source of the site is the [fedora.json](https://github.com/fedora-python/portingdb/blob/master/data/fedora.json). This file is created by a [DNF plugin](https://github.com/fedora-python/portingdb/blob/master/dnf-plugins/py3query.py), that, for every Python package, queries the Fedora rawhide repository and determines package's dependencies, the RPMs the package produces and it's Python 3 support status using a simple heuristic.

{% highlight json %}
{
"python-requests": {
    "deps": [
        "python",
        "python-chardet",
        "python-urllib3",
        "python3"
    ],
    "rpms": [
        "python-requests-2.7.0-7.fc24",
        "python3-requests-2.7.0-7.fc24"
    ],
    "status": "released"
}
}
{% endhighlight %}

## The Fedora & Upstream Status

The fedora.json is complemented by the [fedora-update](https://github.com/fedora-python/portingdb/blob/master/data/fedora-update.yaml), [upstream](https://github.com/fedora-python/portingdb/blob/master/data/upstream.yaml) and [porters](https://github.com/fedora-python/portingdb/blob/master/data/porters.yaml) yamls that contain additional information from the point of view of Fedora, upstream and whoever is doing the porting respectivelly.

They share a similar structure:

{% highlight yaml %}
freeipa:
    status: in-progress
    priority: high
    links:
        repo: https://github.com/encukou/freeipa/tree/py3
{% endhighlight %}

The possible fields are :

- ```status```: if you want to override the status from fedora.json, you can do this here.  Applicable values are idle, in-progress, released, blocked, dropped and unknown.
- ```priority```: if something's blocking your work, raise it up!
- ```links```: link to things like Bugzilla, repo, etc.
- ```note```: various general notes; for instance what is this package replaced by, etc.

All of these are optional.
