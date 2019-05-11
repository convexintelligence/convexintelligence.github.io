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

I picked up a Stack Overflow question yesterday about checking for hash presence that was almost an identical problem. Note that the question is tagged bigdata. In this case, it was “a few million” SHA-256es.

Initially, the user attempted to use both MySQL and Couchbase to solve the problem. Apparently Couchbase used too much memory (presumably using hex encoded keys) and MySQL was too slow, so he tried sharding the table by first nibble, but it still was too slow, so he asked for help.

Two of the answers were (reasonably sensible) suggestions for MySQL. Some schema suggestions and configuration parameters that will help with efficiency.

Another was suggesting some combination of hbase, redis, and cassandra. That’s just… overkill. This is a super small scale problem.

The spec said “several million” of these hashes. I wrote a small test with 50 million hashes. 50e6 * 32 is about a gig and a half of RAM. It’d be unusual to find a computer that couldn’t spare 1.5GB of RAM for such processing. You have to get up to about a billion hashes before it starts to get a little harder.

“But that won’t scale!” you say? An EC2 instance that can hold about 8 billion such hashes in memory costs about $3.50 per hour. By the time you get to that level, you can think about something better anyway.

### But who wants to write some stupid code!

I pointed to the code I’d written in that meeting as an example to get started. It contains both the text -> binary format convert thingy as well as a web server that loads that file into memory and returns an HTTP status that indicates presence. That was a distracted half hour of work.

However, I realized that I’d since written go-hashset. That makes this type of problem much easier.

Below is a complete server using go-hashset and net/http that will return HTTP 204 on a hit and HTTP 410 on a miss given a GET request to /[sha-256].

```go
package main

import (
	"encoding/hex"
	"log"
	"net/http"
	"os"

	"github.com/dustin/go-hashset"
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
Half the code is loading the file and the other half is specific to this HTTP API. It’s easy enough to imagine another protocol if this doesn’t work for you.

### Conclusion

“Big Data” isn’t all that clearly defined, but as a rule of thumb, here are indicators that you’re definitely not working with big data:

* If it fits comfortably in your phone, it’s not big data
* If it fits comfortably in your computer’s RAM, it’s not big data
* If it fits on your laptop’s SSD, it’s not big data
* If it fits on a single hard drive, it’s not big data

One might even argue that if you can fit the data into a single computer, it’s not worth calling it big data, though big data processing tools can benefit even on smaller scale.

In the meantime, enjoy the smaller data in life. It’s fun and easy.
