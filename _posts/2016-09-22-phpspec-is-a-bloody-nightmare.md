---
layout: post
title: "PhpSpec is a bloody nightmare"
description: PhpSpec is a bloody nightmare sometimes  # Description of the post, used for Facebook Opengraph & Twitter
modified: 2016-09-22
category: Programming
tags: [php, testing]
mathjax:
---
At MJM, we use PhpSpec for unit-testing our code. One might argue that these should not be called unit tests since
you write specifications rather than tests. Call it what you will - we can have that argument another day. Today I will
rant about the good and some of the bad things about it.

In a nutshell, PhpSpec helps bring the TDD buzzword to the PHP world. By allowing you to mock shit straight off the bat,
PhpSpec makes it difficult to come up with excuses like _"I wrote the feature but I haven't written any tests yet"_.
Again, I'm not pissed off about that.

In fact, I actually like it because I'm lazy and PhpSpec generates shit for me. I
just make grand statements like _"Oh, I got a Some\Bullshit. And it takes a CrunkJuiceFactory as a constructor
argument. And I can getJuice() from CrunkJuiceFactory anytime I want and it'll give me n number of juice hits"_. That's just
what I told my manager, I haven't written any of it yet. So I'm like shit... better get that scaffolded. So I do:

{% highlight bash %}
bin/phpspec desc Some/Bullshit
{% endhighlight %}

Then PhpSpec just makes a specification for you with nothing much is in it but you can't argue with that since you haven't
done any work yet yourself. But you don't care. You're like **_"Sick! I'm done! I'm gonna just run this test and go home!"_**
because you're all TDD now and they all tell you life is good when you TDD. So, you:

{% highlight bash %}
bin/phpspec run spec/Some/BullshitSpec.php
{% endhighlight %}

Just as you were about to leave, you remember that it's only 9.30am and you kindda need to write the feature still. Argh!
FINE!!!! PhpSpec got yo back though. It already noticed that you've done fuckall so it asks you...

{% highlight bash %}
Some/Bullshit
  10  - it is initializable
      class Some\Bullshit does not exist.

1 specs
1 example (1 broken)
36ms

  Do you want me to create Some\Bullshit for you? [Y/n]
{% endhighlight %}

You take `Y`, and spec gives you your new file and you are ready to write some code. The thought creeps to your mind that
I could have just done the same thing with your IDE with 3 clicks and you don't have to keep chatting to a terminal. You are
correct. But that's not where Spec saves you. Now is the time Spec REAAAALY start to give you bacon. You start mocking the
shit out of your object immediately, winning from the get go:


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

You just noticed that you started describing behaviour of `CrunkJuiceFactory` and making claims about what it will do even
before you wrote anything. You can now start to write your `Bullshit` behaviour without having to write too much other bullshit.
So... it's kindda nice. So you become dependant on it. But with any shit that gets you hooked, you eventually come to
find yourself in that moment of overdose where you are like **_"WTF dawg???"_**.

That moment in PhpSpec happens when you are testing your tests. Spec gets fucking weird here because it's doing shit load of
magic and you just don't understand any of that shit because you never needed to. One such problem is when you are testing
some code that you have wrapped around a `try .. catch` block coz you are a G and you don't run away from your exceptions.

If you cause a PhpSpec error, inside a try catch block in your subject (like making a call to a method that you didn't describe
in the spec) then you are going to spend fucking hours trying to figure why PhpSpec keep running forever and eventually die
without giving you any bacon. Or anything for that matter.

Let me explain better:

{% highlight php %}
<?php

public function getBacon()
{
    try {
        $bacon = $this->baconFactory->getSome();
    } catch (NoBaconException $e) {
        $this->logger->critical("OUT OF BACON!!!! :O");
    }
}

{% endhighlight %}

In the above scenario, if you had forgotten to mock `getSome()` method in your `BaconFactory` object inside your spec then
PhpSpec will throw an Exception right where that 'Undefined method' was called. This means that the exception gets caught
by your subject and not PhpSpec itself. And then PhpSpec hangs. And you have no idea WTF just happened. Nothing in the
terminal. Yeah.

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
    } catch (NoBaconException $e) {
        var_dump($e->getMessage());die;
        $this->logger->critical("OUT OF BACON!!!! :O");
    }
}

{% endhighlight %}

Oh, and don't forget to remove the die statement once you are done debugging.