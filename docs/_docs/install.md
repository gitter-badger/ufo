---
title: Installation
---

### Install with Bolts Toolbelt

If you want to quickly install ufo without having to worry about ufo's dependencies you can simply install the Bolts Toolbelt which has ufo included.

```sh
brew cask install boltopslabs/software/bolts
```

For more information about the Bolts Toolbelt or to get an installer for another operating system visit: [https://boltops.com/toolbelt](https://boltops.com/toolbelt)

### Install with RubyGems

If you prefer to install ufo via RubyGems follow the instructions:

```sh
gem install ufo
```

Or you can add ufo to your Gemfile in your project if you are working with a ruby project.  It is not required for your project to be a ruby project to use ufo.

{% highlight ruby %}
gem "ufo"
{% endhighlight %}

### Dependencies

* Docker: You will need a working version of [Docker](https://docs.docker.com/engine/installation/) installed as ufo shells out and calls the `docker` command.
* AWS: Set up your AWS credentials at `~/.aws/credentials` and `~/.aws/config`.  This is the [AWS standard way of setting up credentials](https://aws.amazon.com/blogs/security/a-new-and-standardized-way-to-manage-credentials-in-the-aws-sdks/).

<a id="prev" class="btn btn-basic" href="/docs/">Back</a>
<a id="next" class="btn btn-primary" href="{% link _docs/structure.md %}">Next Step</a>
<p class="keyboard-tip">Pro tip: Use the <- and -> arrow keys to move back and forward.</p>

