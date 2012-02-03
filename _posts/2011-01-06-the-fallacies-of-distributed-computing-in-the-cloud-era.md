---
layout: post
title: The Fallacies of Distributed Computing Reborn - The Cloud Era
---

*This post was originally written for [New Relic](http://newrelic.com) in January, 2011*

Seventeen years ago, long before Sun Microsystems was putting the <em>"dot in dot-com"</em>, <a href="http://en.wikipedia.org/wiki/L._Peter_Deutsch">L. Peter Deutsch</a>, then a Fellow at Sun, wrote <a href="http://blogs.sun.com/jag/resource/Fallacies.html">The Fallacies of Distributed Computing</a>, a classic and often quoted list of assumptions that programmers may falsely believe when building distributed systems.

The fallacies are:
<ol>
	<li>The network is reliable</li>
	<li>Latency is zero</li>
	<li>Bandwidth is infinite</li>
	<li>The network is secure</li>
	<li>Topology doesn't change</li>
	<li>There is one administrator</li>
	<li>Transport cost is zero</li>
	<li>The network is homogeneous</li>
</ol>

### TL;DR

The Fallacies of Distributed Computing applies directly to web applications, and is especially important when developing applications hosted in the cloud.

### What is a Distributed System?

> A collection of independent computers that appears to its users as a single coherent system<br />&nbsp;&nbsp;- Tanenbaum and Steen: [Distributed Systems: Principles and Paradigms](http://www.cs.vu.nl/~ast/books/ds1/)

> You know you have a distributed system when the crash of a computer you’ve never heard of stops you from getting any work done.<br/>&nbsp;&nbsp; - Leslie Lamport : [Security Engineering: A Guide to Building Dependable Distributed Systems](http://www.cl.cam.ac.uk/~rja14/book.html)

> A distributed system is an application that executes a collection of protocols to coordinate the actions of multiple processes on a network, such that all components cooperate together to perform a single or small set of related tasks.<br/>&nbsp;&nbsp;- [Introduction to Distributed System Design - Google Code University](http://code.google.com/edu/parallel/dsd-tutorial.html")

### Web Apps Are Distributed Systems

Web applications are composed of multiple systems working closely together.  Proxy servers, cache servers, load balancers, web servers, application servers and databases are all likely to be involved in processing a single web request.  Many web applications also leverage other web-APIs. An application may show your [friends on Facebook](http://developers.facebook.com/), allow you to [post to Twitter](https://dev.twitter.com/), or store some files on [Amazon's S3 storage service](http://aws.amazon.com/s3/).

### Web Apps in The Cloud

Ignore the fallacies at your own peril.  For a little while, you might pretend that some of the fallacies don't apply to you when you host applications in datacenters with your own hardware.  Hosting applications in the cloud provides no such grace period.  Failure will happen.  Hosting in the cloud, while being the easiest way to scale a web application, means facing up to these fallacies immediately.

When Netflix moved to the cloud, they were [quick to realize that they could not ignore the fallacies](http://techblog.netflix.com/2010/12/5-lessons-weve-learned-using-aws.html):

> "I knew to expect higher rates of individual instance failure in AWS, but I hadn’t thought through some of these sorts of implications." <br/><br/> "AWS networking has more variable latency. We’ve had to be much more structured about “over the wire” interactions, even as we’ve transitioned to a more highly distributed architecture."<br/><br/>"Co-tenancy can introduce variance in throughput at any level of the stack. You’ve got to either be willing to abandon any specific subtask, or manage your resources within AWS to avoid co-tenancy where you must."<br/><br/> "We’re designing each distributed system to expect and tolerate failure from other systems on which it depends."<br/><br/>&nbsp;&nbsp; -  John Ciancutti, Vice President of Personalization Technology at Netflix, Inc

### The Cloud vs. The Fallacies

The Fallacies of Distributed Computing are even more important to consider when building web applications in the cloud.  While it seems obvious that the cloud highlights these fallacies more than ever (as Netflix noted recently), not everyone agrees.  [Tim Bray](http://www.tbray.org/ongoing) is currently an Android Developer Advocate at Google, former Director of Web Technologies at Sun and editor of the W3C XML specification.  I was surprised to find a post from 2009, [The Web vs. The Fallacies](http://www.tbray.org/ongoing/When/200x/2009/05/25/HTTP-and-the-Fallacies-of-Distributed-Computing), where Tim suggests that the Fallacies are much less important to systems built on the web.  I was even more surprised that he [still agrees with that assertion](http://twitter.com/timbray/status/15548456986742784) when [I asked him](http://twitter.com/briandoll/status/15496214912966656).   Let's see if I can change Tim's mind.

## Disputing The Fallacies: The Cloud Era

### 1. The network is reliable

When you consume web APIs from Facebook, Twitter or Amazon, "the network" is the internet.  How reliable, within a given performance window, is the aggregate of all the web services you interact with?  Tim Bray suggests "if a <code>GET</code> gets a network blowup, just do it again".  Sure, we all do that.  If a page loads really slowly and appears to hang, we just refresh. Do you do that with each of your services and API calls?

The network is not reliable.  When you add up all the points of failure for all the various services and APIs you use, the odds of failure are not just high, they are a given.  Instead, build an app that can function at reduced capacity when a given service is offline.


### 2. Latency is zero

Try as we might, there just isn't a way to improve the speed of light.  Serving customers half-way around the globe affects performance in big ways.  Moving services closer to your customers via cloud availability zones and content delivery networks can help tremendously.  Ignore latency online and you're likely throwing away customers from half the globe.


### 3. Bandwidth is infinite

Not long ago everyone had high-speed internet access at home. (Not everyone, of course.  The [digital divide](http://en.wikipedia.org/wiki/Digital_divide) is real.) Wireless hotspots filled every home and apartment and the web felt fast.  A few years ago this started to change.  Starting with the iPhone, web applications were being used not just over high-speed internet connections, but over phone networks.  Using web apps on mobile devices and tablets reminds us that bandwidth problems did not disappear with the dial-up modem.  Stay mindful of how much data you're shipping across the wire and all your users will be grateful.  For a great read on how The Fallacies apply to mobile development, check out this great series of posts by our friends at [Carbon Five on iPhone Distributed Computing Fallacies](http://blog.carbonfive.com/2010/11/17/fallacy-1-the-network-is-reliabl).

### 4. The network is secure

Network security can be a full-time job for a prominent web business.  [Transport layer security](http://en.wikipedia.org/wiki/Transport_Layer_Security) is great, except when we leak security tokens over insecure connections.  [Firesheep](http://codebutler.com/firesheep) brought some necessary attention last year to the prevalence of private security tokens littering insecure connections, ripe for snooping by the guy in the corner of your favorite coffee shop sipping chai.  The network is not secure, and the more we move our lives online the higher the risk of exposure.  Keep a [security mindset](http://www.schneier.com/blog/archives/2008/03/the_security_mi_1.html)  when developing your apps and keep your users safe.


### 5. Topology doesn't change

One of the biggest benefits of moving applications to the cloud is the ability to change topology at will.  Add new app servers.  Upgrade (and possibly change the architecture of) the CPU on your database server in the middle of the day.  Add reverse proxy servers, cache servers, change CDN providers, migrate availability zones.  Topology changes all the time.  Depending on a static infrastructure design not only limits your ability to respond to change, it increases the likelihood of site-wide outages.

### 6. There is one administrator

Of course there isn't just one administrator.  Even with applications hosted in your own private datacenter, your applications are likely interacting with systems outside your administrative control.  These systems may have performance, availability or security issues that you have no direct influence over.  Staying mindful that these systems are beyond your control can help you ensure they have minimal impact on your services when they are unresponsive.

### 7. Transport cost is zero

Not only is transport cost not zero, it's priced like any other commodity and can be purchased per transaction, per gigabyte, per compute hour, etc.  Cloud storage costs (and transport of that storage) are a major component of application hosting costs, just ask anyone doing video streaming online how close to zero their transport costs are.

### 8. The network is homogeneous

This fallacy was added to the original seven by James Gosling, creator of Java, in 1997.  The best part of this fallacy is that it might actually be true.  Almost.  Between [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) and [JSON](http://en.wikipedia.org/wiki/Json) APIs, have you ever had to think about the implementation of an external service, beyond satisfying your curiosity?  You're much more likely to see [libraries producing invalid messages](http://www.google.com/#sclient=psy&amp;q=invalid+json) than you are operating systems playing a role.

### Building performant distributed web applications is hard

The Fallacies were coined 17 years ago, but they apply just as well today.  Back then folks might have been fooled into actually believing them, but we're smarter than that, right?  Knowing what gotchas await our web applications, most notably those hosted in the cloud, can help us build better, faster and more performant applications.  Isn't that what we all want?
