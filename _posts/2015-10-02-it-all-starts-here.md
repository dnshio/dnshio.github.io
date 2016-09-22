---
layout: post
title: "It all starts here"					# Title of the post
description: The end of the begging 		# Description of the post, used for Facebook Opengraph & Twitter
headline: Some headline				# Will appear in bold letters on top of the post
modified: 2015-10-02				# Date
category: personal
tags: []
image: 
  feature: some-image.jpg
comments: true
mathjax:
---
For years I've been meaning to create this without really making any progress. Not that I didn't know how to, but mostly 
because I couldn't figure out how best to. On many occasions, I've set about creating my own personal blog only to 
give up in frustration of balancing between the principle of not re-inventing the wheel but becoming sick of existing 
tools available for blogging. Ultimately I believe I've found the right tool in Jekyll where I can write my blog using 
an IDE so that I look like working.

In the coming weeks and months, I hope to bring to you all the news as I see it, whether personal or global. I also hope 
to use this site to document some of the programming discussions and discoveries that I make along the way so this site 
might well become a source of information at some distant future. 

With all that being said, I'm off to get some lunch. 


{% highlight php startinline=true %}
<?php
try {
    $response = $request->send();
    $responseBody = json_decode($response->getBody(true), true);

    $this->trafficLimiter->setRequestsRemaining(
        (int) $response->getHeader('X-Requests-Remaining')
    );
    $this->trafficLimiter->setCoolOffTimeInSeconds(
        (int) $response->getHeader('X-Rate-Limit-Ttl')
    );

} catch (ClientErrorResponseException $exception) {
    $this->logger->error($exception->getMessage());
}
{% endhighlight %}


Bye !

