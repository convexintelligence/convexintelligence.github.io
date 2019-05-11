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

Around 2006, I gave a presentation about "Big Data" at our organization to a group of engineers and scientists coming from the Bottom Up school of thought. I was the primary "Data Dude" up until that point and when we started expanding rapidly in 2006, my boss wanted others to also take data driven discovery seriously. Before beginning my career there, I was heavily into dynamic modeling. So, ODE's, DAE's, PDE's dominated my day to day life along with the occasional stochastic process simulations. Data guided model development is a Top Down approach with more inherent uncertainties compared to deterministic modeling. But, when done correctly (and by that I mean conservatively), it is the most potent thing in the modeling arsenal. Equipped with patterns/relationships gleaned/infered from data, one can easily build a bottom up strategy to effectively quantify dynamics. For an engineer, this is where the secret sauce is! Top Down leads to Bottom Up leads to model goal. Such a Middle Out approach has always worked out best in my experience with engineering simulations. 

The presentation was well received by everyone and I was asked to write a very brief article capturing the gist of my presentation for internal circulation. The following is what I wrote and it is from Autumn 2006. It was based on the analysis of a biological sample dataset and the process flow that sort of indicate whether the data you are working with is big or not. Trust me folks, back in 2006, the definition of big data was not clear and there was more confusion about the term than it is now. Anywaw, read on ....

**The Big Data Hype - Is it worth it? By Prem K. Murugan, Research Engineer, Model Development Division, BVT, CVT, IST & ISR**

### Are you really working with "Big Data"?

A while back, I managed to convince a research acquaintance of mine to give access to some static medical data in the form of a list of expression levels across a select sample. You could do some preliminary analysis with highly expensive tools that would connect to a database and let you annotate the data first. But the programs were slow and database errors were common. Frankly, the database was a stretch and I was perplexed that a unique query of an amount of immutable dataset would even warrant a database.

I set up a small server pretty quickly that could easily load the data into memory for a few seconds and throw a response ridiculously faster with a pitch perfect horizontal scalability.

### Scope Planning

The data I had access to was nearly 670MB. I have tabs in Chrome right now that are using more memory than this, yet people deployed multi-tier infrastructure to address simple annotation queries . They are complex, sluggish and extremely fragile.

### Do I really need a fucking Database?

This brings me to my motivation for writing this. People have gone a little bit overboard with thinking up big solutions to small problems. I’m not trying to pick on any particular user, but I did have an example that helps make the point pretty clearly.

