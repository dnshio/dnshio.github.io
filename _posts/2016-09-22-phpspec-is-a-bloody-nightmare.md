---
layout: post
title: "PhpSpec is a bloody nightmare"
description: PhpSpec gives with one hand but takes with both  # Description of the post, used for Facebook Opengraph & Twitter
modified: 2016-09-22
category: Programming
tags: [php, testing]
mathjax:
---
Where I work, we use PhpSpec for unit-testing our code. One might argue that these should not be called unit tests since
you write specifications rather than tests. Call it what you will - we can have that argument another day. Today I will
rant about the good and some of the bad things about it.

In a nutshell, PhpSpec helps bring the TDD buzzword to the PHP world. By allowing you to mock shit straight off the bat,
PhpSpec makes it difficult to come up with excuses like _"I wrote the feature but I haven't written any tests yet"_.
Again, I'm not pissed off about that.

In fact, I actually like it because I'm lazy and PhpSpec generates shit for me. I
just make grand statements like _"Oh, I got a Some\Bullshit. And it takes a CrunkJuiceFactory as a constructor
argument. And I can getJuice() from CrunkJuiceFactory anytime I want and it'll give me n number of juice hits"_. But I
haven't written any of that behaviour yet. Better get that scaffolded... So I do:

{% highlight bash %}
bin/phpspec desc Some/Bullshit
{% endhighlight %}

Then PhpSpec just makes a specification for you with nothing much is in it but you can't argue with that since you haven't
done any work yet yourself. But you don't care. This is TDD so you run the test anyway:

{% highlight bash %}
bin/phpspec run spec/Some/BullshitSpec.php
{% endhighlight %}

Oh look! PhpSpec wants to help you create the actual test subject!

{% highlight bash %}
Some/Bullshit
  10  - it is initializable
      class Some\Bullshit does not exist.

1 specs
1 example (1 broken)
36ms

  Do you want me to create Some\Bullshit for you? [Y/n]
{% endhighlight %}

You take `Y`, and spec gives you your new file and you are ready to write some code. At this point, I know the IDE warriors
are up on my case pointing out that one could achieve the same with 3 clicks and you don't have to keep chatting to a terminal. You are
correct. But that's not where Spec saves you. PhpSpec gives you the ability to focus on TDD, writing your subject and mocking
the behaviour of its dependencies right from the start.

For example, you can just scaffold out `CrunkJuiceFactory` object just like your `Bullshit` object. And then you can mock
return values of methods in `CrunkJuiceFactory` so that you can just focus on writing the actual behaviour of `Bullshit`:


{% highlight php %}
<?php
namespace spec\Some;

use PhpSpec\ObjectBehavior;
use Prophecy\Argument;

class BullshitSpec extends ObjectBehavior
{
    public function let(CrunkJuiceFactory $cjFactory) {
        $cjFactory->getJuice()->willReturn(5);
        $this->beConstructedWith($cjFactory);
    }

}

{% endhighlight %}

You just noticed that you started describing behaviour of `CrunkJuiceFactory` and making claims about what it will do before
you even thought about implementing it. This is great for productivity because you don't have to write both classes in parallel.

But how does that even work?? This witchcraft makes you productive but unless you understand the underlying mechanics of
the framework, you will run in to problems. Essentially, PhpSpec makes heavy use of another buzzword. **Reflection**.

Reflection allows PhpSpec to get a description of the object and the dependencies that you are trying to test. And using
this information, PhpSpec creates stunt doubles of your objects which it pushes through your test subject. And this stunt
double essentially intercepts calls made from your test subject to it and then returns values that you have mocked. This
is what you did when you described `$cjFactory->getJuice()->willReturn(5);`.

All this stuff sounds great! Why the damn rant? Well, the problem is that this makes it very difficult to test your tests.
Spec gets weird when you are testing some code that you have wrapped around a `try .. catch` block.

If you cause a PhpSpec error, inside a try catch block in your subject (like making a call to a method that you didn't describe
in the spec) then you are going to spend hours trying to figure out why PhpSpec keep running forever and eventually die
without giving you any bacon. Or anything for that matter.

Let me explain better:

{% highlight php %}
<?php

public function getBacon()
{
    try {
        $bacon = $this->baconFactory->getSome();
    } catch (\Exception $e) {
        $this->logger->critical("OUT OF BACON!!!! :O");
    }
}

{% endhighlight %}

In the above scenario, if you had forgotten to mock `getSome()` method in your `BaconFactory` object inside your spec then
PhpSpec will throw an Exception right where that 'Undefined method' was called. This means that the exception gets caught
by your subject and not PhpSpec itself. And then PhpSpec hangs. And you have no idea WTF just happened. Nothing in the
terminal. Just hangs. If you ran it with php -d memory_limit=-1 then it hangs even longer. Possibly until the end of the
world.

**So how do you deal with this?**

If you come across this sorry situation in your life, try and narrow down exactly which spec is hanging. And then you will
probably notice that your subject has a try catch block. Then stick a var_dump in the catch block with the exception message
and die. It'll show you the message on your terminal so you can move forward.

{% highlight php %}
<?php

public function getBacon()
{
    try {
        $bacon = $this->baconFactory->getSome();
    } catch (\Exception $e) {
        var_dump($e->getMessage());die;
        $this->logger->critical("OUT OF BACON!!!! :O");
    }
}

{% endhighlight %}

Oh, and don't forget to remove the die statement once you are done debugging.