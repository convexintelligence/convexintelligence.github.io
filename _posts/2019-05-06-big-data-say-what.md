---
layout: post
title: Big Data - Say What?
subtitle: My data is "bigger" than your data!
date: 2019-05-06 21:22:49
author: Prem
---

<div class="block">
          <left><img src="{{ site.baseurl }}/img/bd2.png" alt="Img" style="width:640px;"/></left>
          </div>
          
# Background


Around 2006, I gave a presentation about "Big Data" at our organization to a group of engineers and scientists coming from the Bottom Up school of thought. I was the primary "Data Dude" up until that point and when we started expanding rapidly in 2006, my boss wanted others to also take data driven discovery seriously. Before beginning my career there, I was heavily into dynamic modeling. So, ODE's, DAE's, PDE's dominated my day to day life along with the occasional stochastic process simulations. Data guided model development is a Top Down approach with more inherent uncertainties compared to deterministic modeling. But, when done correctly (and by that I mean conservatively), it is the most potent thing in the modeling arsenal. Equipped with patterns/relationships gleaned/infered from data, one can easily build a bottom up strategy to effectively quantify dynamics. For an engineer, this is where the sauce is! Top Down leads to Bottom Up leads to model goal. Such a Middle Out approach is best modeling strategy. 

The presentation was well received by everyone and I was asked to write an article capturing the gist of my presentation for internal circulation. Ths following is what I wrote and it is from Autumn 2006. Read on folks.....

### Define Big Data

A while back, there was a leak of a LinkedIn password data in the form of a list of unsalted SHA-1 hashes. A few sites had password check tools up such that you could provide your password or a hash of it and it’d tell you whether it was found in the leaked information.

These sites were all really slow, and would sometimes report database errors. I found it curious that anyone would even consider a database for a fixed-size single record lookup of a small amount of immutable data.

I downloaded the data set and played with it during a meeting. In about a half hour, I had a small server that could load the data set into memory in a couple seconds and serve responses from memory stupidly fast an with perfect horizontal scalability.

