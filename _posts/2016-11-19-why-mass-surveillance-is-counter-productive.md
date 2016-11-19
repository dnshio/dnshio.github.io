---
published: true
layout: post
description: 'Mass surveillance is not only useless, it''s also dangerous'
modified: 2016-11-19T00:00:00.000Z
category: Security
tags:
  - security
  - surveillance
---
Last Thursday(17th Nov 2016), The [UK Parliament approved a bill called Investigatory Powers Act](https://www.theguardian.com/world/2016/nov/19/extreme-surveillance-becomes-uk-law-with-barely-a-whimper) which legalises a plathora of digital spying activities by government agencies. This bill is also commonly known as "The Snoopers Charter". The bill grants the following powers:

## Equipment interference by government agencies becomes legal
Upon obtaining a warrant, government agencies may hack into computers, networks, mobile devices, servers or any other connected or offline devices. 
This could include downloading data from a mobile phone that is stolen or left unattended, or software that tracks every keyboard letter pressed being installed on a laptop.

Whlist this is nothing new, this is the first time the practice has been made legal in a western democracy. Given that a warrent is required before any attacks, many people would be fine with this law. But... 

## Bulk hacking and collecting bulk data sets also becomes legal
This is where things start to get a little intrusive. "Bulk hacking" grants government agencies to perform proximity attacks on networks and devices in bulk, and almost indiscriminately. For example, it would become legal for government agents to hack your company's network if they think one of your colleagues is emailing ISIS. Not only that, it's also legal for them to keep whatever data they might find about YOU even though you had nothing to do with it. 

## IP Bill and web logs
Internet service providers are now oblidged to store internet history data for all their customers for a minimum of 12 months. Internet history data logs will be made up of websites that you may have visited and devices that you have connected with. Government agencies are to be given full access to all of this data without any need for a warrant. 

## OK so what? What's wrong with all this?
Lets not worry about *equipment interference* since this is quite similar to other breaking and entering laws that many think is fair. However, very few would find indiscriminate attacks on private data to be fair.

But I'm not terribly fussed about the fairness of this bill, actually. What I'm concerned about is how ineffetive are these policies at doing what the government claim they do: **counter-terrorism and provide security**. Before going further, let me introduce some security engineering jargon:

1. **Policy** (or the objective goal)
2. **Mechanism** (ciphers, access controls, hardware tamper-resistance and other machinery that you assemble in order to implement the policy)
3. **Assurance** (the amount of reliance you can place on each particular mechanism)
4. **Incentive** (the motive for people to either guard or try and defeat the policy)

Snoopers Charter is a collection of **_ineffective policies_** backed by **_weak mechanisms_** designed to provide the **_illusion of assurance_**. 

These policies are ineffective mainly due to the weak mechanisms. IP bill can be defeated quite easily [by proxying through a VPN](http://www.pcadvisor.co.uk/how-to/internet/how-use-vpn-set-up-vpn-for-private-browsing-3466190/), rendered useless when people are using geo busting services like [Hola](http://www.hola.org) or anonymizers such as [Tor onion router](https://www.torproject.org/projects/torbrowser.html.en). In the case of geo busting, people are unwittingly helping scramble online traffic just to be able to watch the American catelog of Netflix at the risk of potentially having to take responsibilty for someone else's online activity. This fact alone makes web log storage one of the most retarded policies ever introduced.

**But it should help catch at least _some_ criminals, yes?**
Nope. At least not the kind of sophisticated criminals that we all should be worried about. End-to-end encryption is now pretty wide-spread that even mainstream messaging service WhatsApp encrypts messages during transit. Someone who has a vested interest in maintaining annonymity and communicating secretly can go about doing so with relative ease using some free to use software like the Tor browser and [PGP encryption](https://en.wikipedia.org/wiki/Pretty_Good_Privacy). So what's actually happening is that **criminals and terrorists can go about their business without having to change _anything_ while ISPs will have to spend millions of pounds maintaing web-logs of their customers**. It is almost certain that ISPs will pass on this cost to their customers. But what is dangerous about this is that there will no doubt be a demand in the digital blackmarket for all this web-log data. Yahoo were the most recent company to come out clean [about how successful they were at securing their customer data](https://www.theguardian.com/technology/2016/sep/23/yahoo-questinos-hack-researchers). When it comes to privacy, it's very difficult to put the genie back in the bottle. So the less data that is stored about you, the better your privacy. 

## So why would the government bring about such ineffective policies?
Because that's what governments do. These policies have more to do with public perception than being objective. I.e these policies are designed to give people the illusion security whilst not really achiveing it. This sort of thing happens all the time. Politicians bring these up in order to satisfy certain groups of voters.

For example, since the 9/11 attacks the Transport Security Administration in USA has spent $14.7 billion on aggressive passenger screening, which is fairly ineffective since well below half of the weapons taken through screening (accidentally or for testing purposes) were picked up, while $100m spent on reinforcing cockpit doors would remove most of the risk. 

So... I ask you to make sure that you play your part in having these policies binned. Learn and use traffic scramblers, annonymizers and encryption whenever possible. 



