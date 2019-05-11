---
layout: post
title: What is Big Data?
subtitle: My data is "bigger" than your data!
date: 2019-05-06 21:22:49
author: Prem
---

<div class="block">
          <left><img src="{{ site.baseurl }}/img/bd2.png" alt="Img" style="width:640px;"/></left>
          </div>
          
# A little Background

Around 2006, I gave a presentation about "Big Data" at our organization to a group of engineers and analysts, who had visited us from our sister lab in the USA. I was the primary "Data Dude" at our facility up until that point and I was leaving the company in two months. Naturally, my boss wanted to pass on some of what I have learned over the years to the visiting team. Before beginning my career there, I was heavily into dynamic modeling. So, ODE's, DAE's, PDE's dominated my day to day life along with the occasional stochastic process simulations. Data guided model development is a Top Down approach with more inherent uncertainties compared to deterministic modeling. But, when done correctly (and by that I mean conservatively), it is the most potent thing in the modeling arsenal. Equipped with patterns/relationships gleaned/infered from data, one can easily build a bottom up strategy to effectively quantify dynamics. For an engineer, this is where the secret sauce is! Top Down leads to Bottom Up leads to model goal. Such a Middle Out approach has always worked out best in my experience with engineering simulations. 

The presentation was well received by everyone and I was asked to write a very brief article capturing the gist of my presentation for internal circulation. The following is what I wrote in Spring 2006. It was based on the analysis of a biological dataset sample and the process flow that sort of indicate whether the data you are working with is big or not. Back in 2011, the definition of big data was not clear and there was more confusion about the term than it is now. Anyway, read on ....

**The Big Data Hype - Is it worth it? By Prem K. Murugan, Research Engineer, Model Development Division, BVT, CVT, WIW, ISR & IST**

### Are you really working with "Big Data"?

In the Winter of 2005, a research acquaintance gave me access to closed source clinical data to see if we could exploit it to find some interesting interactions with the new method I was developing. It was great, since I knew the data that I had banked on was going to be delayed by another six months and what better way to validate your algorithm with a similar dataset. The data was a mess and required a lot of preprocessing to render it analysis-ready in the first place. Most importantly, it was not annotated making it completely useless. So, the first thing to do would be sanitize and structure the data in a way that relevant entries could be checked against available information in a few sites that would let you annotate the data. Basically, we were looking for the presence of a certain element in the data. But those sites were painstakingly slow and I always encountered database errors. Frankly though, I was perplexed that a fixed-size unique query of an amount of immutable dataset would even warrant a database.

So, I set up a small server pretty quickly that could easily load the relevant entries into memory for a few seconds and throw a response ridiculously faster with a pitch perfect horizontal scalability.

### Scope Planning

The data I had was around 650MB. That was it. My firefox browser uses more memory than this, yet people deployed multi-tier infrastructure to address distinctive fucking queries . They are complex, sluggish and extremely fragile.

### Do I really need a fucking Database?

Now, this is exactly the reason why I am writing this. People have gone a little overboard with thinking up big solutions to small problems. More so, since the word "Big Data" started to become a big part of the technology landscape. I’m not trying to pick on any particular person, but I have an example that helps make the point pretty clearly.

I happened to find a question yesterday in one of my usual hangouts about data annotation that was exactly the same problem that I had written earlier. The question was tagged bigdata. In this case, it was “1.2-1.5 gigs". Initially, the gentleman attempted to use both MySQL and NoSQL to solve the problem. Apparently NoSQL used too much memory (presumably using hex encoded keys, duh!) and MySQL was too slow, so he table sharding by first nibble, but it still was too slow, so he asked for help. I liked a couple of answers that were (reasonably sensible) suggestions for MySQL. Some schema improvements and configuration parameters that will help with efficiency. Another was suggesting some combination of hbase and cassandra. That there ladies and gentlemen is a fucking overkill! This is a super small scale problem.

The spec said “1.2-1.5gigs". I case tested a scenario that required close to 2 gigs of RAM. It’d be unusual to find a computer that couldn’t spare 2GB of RAM for such processing. You have to get up to considerably more gigs before it starts to get a little harder. "This is all fine, but how do you scale this shit!“ you say? Last time I checked, an EC2 instance that can hold about that kind of data in memory costs about $3.50 per hour. By the time you get to that level, you can think about something better anyway.

### I hate coding myself you moron!

I gave him the server code I had written earlier to get him interested. It contains both the data structuring routine as well as a web server that loads that file into memory and returns an HTTP status that indicates the presence of the element of interest. Honestly, it would take no more than half and hour to code it.

### Conclusion

“Big Data” still lacks a proper definition, but as a rule of thumb, here are a few indicators that you’re definitely not working with big data:

* If it fits comfortably in your phone, it’s not big data
* If it fits comfortably in your computer’s RAM, it’s not big data
* If it fits on your laptop’s SSD, it’s not big data
* If it fits on a single hard drive, it’s not big data

One might even argue that if you can fit the data into a single computer, it’s not worth calling it big data, though big data processing tools can benefit even on smaller scale. In the meantime, enjoy the smaller data in life. It’s fun and easy.
