---
title: Simple Slack interface for your Python application with Yasuf
layout: post
---

Slack is now pretty ubiquitous and having the ability to control your Python services via Slack can be nice. With [Yasuf](https://github.com/sYnfo/Yasuf) you can do that with just a single decorator.

## Motivation
I recently needed to add Slack control to a service that manages AWS EC2/GCE instances in response to changing load. I wanted something that could be quickly plugged into an existing code base and just work.

Nothing I found was quite as simple as I'd hoped, and that's how Yasuf was born.

## Usage
In my specific case I have a few functions that launch, terminate and list instances. For example the launch function looks like this:

{% highlight python %}
def launch_instances(count=1):
    (...)
    print('Launched %i instances.', count)
    return instance_ids
{% endhighlight %}

To launch instances via Slack and get all the text output and the return value, I simply decorate the function with `yasuf.handle`.

{% highlight python %}
yasuf = Yasuf(<slack-token>, channel='ops')

@yasuf.handle('launch ([0-9]+)', types=[int])
def launch_instances(count=1):
    (...)
    print('Launched %i instances.', count)
    return instance_ids
{% endhighlight %}

And run Yasuf when executing the service.

{% highlight python %}
if __name__ == '__main__':
    yasuf.run_async()
    (...)
{% endhighlight %}

And that's it! Typing 'launch 5' in the 'ops' channel will now launch 5 instances and give you back the logs.

## Contributing
The github repo is at [sYnfo/Yasuf](https://github.com/sYnfo/Yasuf) and I very much welcome both new issues and pull requests. :)
