---
layout: post
title: "Solved: Vagrant can't download any boxes"
description: OSX Sierra and the latest version of Vagrant have cocked up cURL
modified: 2016-11-21
category: Development
tags: [vagrant]
mathjax:
---
If you've just installed Vagrant (1.8.7) on OSX Sierra then you might come across an error message similar to the following when trying to run `vagrant up`    

{% highlight bash %}    
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'hashicorp/precise64' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
The box 'hashicorp/precise64' could not be found or
could not be accessed in the remote catalog. If this is a private
box on HashiCorp's Atlas, please verify you're logged in via
`vagrant login`. Also, please double-check the name. The expanded
URL and error message are shown below:

URL: ["https://atlas.hashicorp.com/hashicorp/precise64"]
Error:
{% endhighlight %}

This happens because vagrant seems to have shipped with some cUrl implementation that doesn't work. Disabling Vagrant cUrl and re-enabling system cUrl solves this problem. 


`sudo mv /opt/vagrant/embedded/bin/curl /opt/vagrant/embedded/bin/curl_orig`    
`sudo mv /opt/vagrant/embedded/bin/curl-config /opt/vagrant/embedded/bin/curl-config_orig`    

symlink the good one from brew       
`sudo ln -s /usr/bin/curl /opt/vagrant/embedded/bin/curl`    
`sudo ln -s /usr/bin/curl-config /opt/vagrant/embedded/bin/curl-config`    

Running `vagrant up` should work as normal. 