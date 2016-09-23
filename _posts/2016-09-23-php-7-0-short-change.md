---
layout: post
title: "PHP 7.0 short-change"
description: PHP 7 gave us a lot. But it still had that PHP knack for falling just short
modified: 2016-09-23
category: Programming
tags: [php]
mathjax:
---
So, It's been almost a year now since PHP 7 had been released. I have to say that regardless of the tone of this article's
title, I am over the moon about this major version. I think it's really come a long way from the old "Personal Home Page"
laughing-stock. And I'm not even talking about the [PHP 7 vs Python 3 benchmark game](http://benchmarksgame.alioth.debian.org/u64q/php.html).

No, I mean it had actually become a decent language and I am not that sad about having to work with it day in day out. I
sometimes even admit at parties to being a PHP developer.

PHP 5.6 was already pretty good. We had short syntax arrays, consistent and standard compliant JSON serialization/de-serialisation,
Traits, Reflection and so on. So the big thing apart from all the _we're faster than python_ performance improvements has
to be the inclusion of optional typing for scalars (again, PHP 5.6 fell a little short on this one).

I love it! Now I don't have to spend time type checking at the beginning of every method like we used to:

{% highlight php %}
<?php
function multiply($x, $y)
{
  if (!is_numeric($x) || !is_numeric($y)) {
    throw new \InvalidArgumentException("Both X and Y has to be a number");
  }
  return $x * $y;
}
{% endhighlight %}

Look at that shit! Anyone who says dynamic typing makes it easier to write code probably doesn't type check their input arguments.
In the above example, 3 lines of code went in to type checking and the actual business logic was a one-liner. So I'm glad
that PHP 7 allows us to type hint scalars. Another bonus is that we can now declare the return type as well. This now cuts
down the amount of code on the calling method. Check what the same example looks like on PHP 7:

{% highlight php %}
<?php
function multiply(int $x, int $y) : int
{
  return $x * $y;
}
{% endhighlight %}

Beautiful! And this is great when taking two integers and returning one. But people pay me money for my code so life is never
that simple. I have to do things from time to time that isn't discussed in the release notes. Like return null sometimes
(or all the time for that matter). Check this:

{% highlight php %}
<?php
function getOneLemon() : Lemon
{
  if (count($this->lemons)) {
      return $this->lemons[0];
  }
  return null;
}
{% endhighlight %}

Looks pretty reasonable but the code above will throw a fatal because you promised to return a `Lemon` but sometimes you can't.
One way around it was to get rid of the return type declaration and maybe describe the return types in the docblock so your
IDE knows what you are up to. I mean... we just lost all the power that this feature was supposed to give us.

Another way was to throw an exception when you can't return the type you promised. This is a little bit better but if you
really think about it, you are relying on a construct designed to signal failures and errors as a part of the logic flow.
This makes me uncomfortable. If I go to a shop and ask for some sugar I don't want the shop keeper to start shutting down
the shop when he notice that he doesn't have any sugar to sell. No is a perfectly acceptable answer.

But there is light at the end of the tunnel. An RFC was accepted and this issue is ironed out in the next minor version upgrade
(7.1) - along with a heap of other improvements. The official released is set to be in Nov this year. So in 7.1 the last
example would look like this:

{% highlight php %}
<?php
function getOneLemon() : ?Lemon
{
  if (count($this->lemons)) {
      return $this->lemons[0];
  }
  return null;
}
{% endhighlight %}

Boom. The question mark in front of the `Lemon` indicates that it's optional. That means you can return null as well as
`Lemon` (but no other type). Pretty solid. Another thing - you can also return a new `void` type. Looking forward to that!
