---
layout: post
title:  "Local gem repository"
date:   2015-02-21 01:44:05
categories: posts
---

Suppose we have a machine not connected to a network and a bunch of ".gem" files and we need to make Bundler to use them instead of fetching https://rubygems.org. Let's see how it can be managed on an example.

## **The way it works**

For simplicity let's assume that we need to install just 1 gem, for example gem "colorize". We downloaded a gemified version (with ".gem" extension) using the command 

{% highlight bash %}
gem fetch colorize
{% endhighlight %}

and got in the result a file called colorize-0.7.5.gem.

Next, execute in a shell:

{% highlight bash %}
sudo mkdir /var/gems
sudo mv colorize-0.7.5.gem /var/gems/
cd /var/gems
export GEM_HOME=/var/gems
gem install /var/gems/colorize-0.7.5.gem
gem server -d /var/gems
{% endhighlight %}

After running "gem server" command you should get an output like:

{% highlight bash %}
Server started at http://0.0.0.0:8808
Server started at http://[::]:8808
{% endhighlight %}

Next create a Gemfile:

{% highlight ruby %}
source "http://127.0.0.1:8808"
gem 'colorize'
{% endhighlight %}

And now you can run 

{% highlight bash %}
bundle install
{% endhighlight %}

and it will load gems from the /var/gems directory.

## **The way it should work**

Actually it should be possible to do it in another way - without installing gems, but at the moment this article is being written it doesn't work for some reason. So what we would need to do with this another approach is:

{% highlight bash %}
sudo mkdir /var/gems
sudo mv colorize-0.7.5.gem /var/gems/
cd /var/gems
sudo gem install builder 
sudo gem generate_index -d /var/gems
{% endhighlight %}

Gem "builder" is required to successfully run "gem generate_index" command generating a gem index, so we would have to install it as it's described in the first approach above assuming that the machine is not connected to network.

Next create a Gemfile

{% highlight ruby %}
source "file:///var/gems"
gem 'colorize'
{% endhighlight %}

Now you should be able to run "bundle install".

