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

Around 2011, I gave a presentation about "Big Data" at our organization to a group of engineers and analysts, who had visited us from HQ. I was the primary "Data Dude" at our facility up until that point and I was leaving the company in two months. Naturally, my boss wanted to pass on some of what I have learned over the years to the visiting team. Before beginning my career there, I was heavily into dynamic modeling. So, ODE's, DAE's, PDE's dominated my day to day life along with the occasional stochastic process simulations. Data guided model development is a Top Down approach with more inherent uncertainties compared to deterministic modeling. But, when done correctly (and by that I mean conservatively), it is the most potent thing in the modeling arsenal. Equipped with patterns/relationships gleaned/infered from data, one can easily build a bottom up strategy to effectively quantify dynamics. For an engineer, this is where the secret sauce is! Top Down leads to Bottom Up leads to model goal. Such a Middle Out approach has always worked out best in my experience with engineering simulations. 

The presentation was well received by everyone and I was asked to write a very brief article capturing the gist of my presentation for internal circulation. I was quite interested in Cryptography at that time and my llustration of Big Data was based on it, since I couldn't talk about my main project (more on that later). Also, I wanted to showcase my Go code to analyze this problem. The following is what I wrote in Spring 2011. It was based on the analysis of some password dataset and the process flow that sort of indicate whether the data you are working with is big or not. Trust me folks, even back in 2011, the definition of big data was not clear and there was more confusion about the term than it is now. Anyway, read on ....

**The Big Data Hype - Is it worth it? By Prem K. Murugan, Senior Analyst, Model Development Division, DM, IST & ISR**

### Are you really working with "Big Data"?

In the Winter of 2011, I played around with SHA-1 (unsalted) hashes coming from a simulated password leak scenario. A security team had built a few sites with password check tools that would take your password or it's hash and let you know if your credentials were found in the leaked information. But these sites were slow and database errors were common (sorry SecRes). Frankly, I was perplexed that a fixed-size unique query of an amount of immutable dataset would even warrant a database.

I set up a small server pretty quickly that could easily load the data into memory for a few seconds and throw a response ridiculously faster with a pitch perfect horizontal scalability.

### Scope Planning

The data I had access to had 7.5 million hashes. As you know, SHA-1 hashes are 20 bytes. So, 7.5 million times 20 equals 150MB of data. That was it. My firefox browser uses more memory than this, yet people deployed multi-tier infrastructure to address simple yes or no queries . They are complex, sluggish and extremely fragile.

### Do I really need a fucking Database?

Now, this is exactly the reason why I am writing this. People have gone a little overboard with thinking up big solutions to small problems. More so, since the word "Big Data" started to become a big part of the technology landscape. I’m not trying to pick on any particular person, but I have an example that helps make the point pretty clearly.

I happened to find a question yesterday in one of my usual hangouts about hash presence checks that was exactly the same problem that I had illustrated earlier. The question was tagged bigdata. In this case, it was “a few million” SHA-256es. Initially, the gentleman attempted to use both MySQL and NoSQL to solve the problem. Apparently NoSQL used too much memory (presumably using hex encoded keys, duh!) and MySQL was too slow, so he table sharding by first nibble, but it still was too slow, so he asked for help. I liked a couple of answers that were (reasonably sensible) suggestions for MySQL. Some schema improvements and configuration parameters that will help with efficiency. Another was suggesting some combination of hbase, redis, and cassandra. That there ladies and gentlemen is a fucking overkill! This is a super small scale problem.

The spec said “several million” of these hashes. I wrote a small test case scenario with 50 million hashes. 50e6 * 32 is about one and a half gigs of RAM. It’d be unusual to find a computer that couldn’t spare 1.5GB of RAM for such processing. You have to get up to about a billion hashes before it starts to get a little harder. "This is all fine, but how do you scale this shit!“ you say? Last time I checked, an EC2 instance that can hold about 8 billion such hashes in memory costs about $3.50 per hour. By the time you get to that level, you can think about something better anyway.

### I hate coding myself you moron!

I gave him the server code I had written earlier to get him interested. It contains both the text -> binary format conversion thingy as well as a web server that loads that file into memory and returns an HTTP status that indicates presence of a hash. Honestly, it would take no more than half and hour to code it.

Fast forward a few weeks, I had written a stupid hashset. This actually simplifies the discussed problem. Below is a complete server using hashset and net/http that will return HTTP 204 on a hit and HTTP 410 on a miss given a GET request to /[sha-256].

```go
package main

import (
	"encoding/hex"
	"log"
	"net/http"
	"os"

	"/home/sage/hacks/go-hashset"
)

const (
	hashSize   = 32
	listenAddr = ":8080"
)

func loadFile(fn string) *hashset.Hashset {
	f, err := os.Open(fn)
	if err != nil {
		log.Fatalf("Error opening hash file: %v", err)
	}
	defer f.Close()
	hs, err := hashset.Load(hashSize, f)
	if err != nil {
		log.Fatalf("Error loading hashes: %v", err)
	}

	return hs
}

func main() {
	hs := loadFile("hashes.bin")

	http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
		b, _ := hex.DecodeString(req.URL.Path[1:])
		if len(b) == hashSize && hs.Contains(b) {
			w.WriteHeader(204)
		} else {
			w.WriteHeader(410)
		}
	})

	log.Printf("Listening on %v", listenAddr)
	log.Fatal(http.ListenAndServe(listenAddr, nil))
}
```
As you see, loading the hashes is taken care of by half the program and the other half is specific to this HTTP API. It’s easy to come up with another protocol to solve this problem as well.

### Conclusion

“Big Data” still lacks a proper definition, but as a rule of thumb, here are a few indicators that you’re definitely not working with big data:

* If it fits comfortably in your phone, it’s not big data
* If it fits comfortably in your computer’s RAM, it’s not big data
* If it fits on your laptop’s SSD, it’s not big data
* If it fits on a single hard drive, it’s not big data

One might even argue that if you can fit the data into a single computer, it’s not worth calling it big data, though big data processing tools can benefit even on smaller scale. In the meantime, enjoy the smaller data in life. It’s fun and easy.
