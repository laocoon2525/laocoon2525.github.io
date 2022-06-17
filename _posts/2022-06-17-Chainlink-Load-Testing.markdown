---
layout: post
title:  Chainkink load testing
date:   2022-06-17 14:54:41 -0700
categories: Gatling Load Testing Chainlink
---

Recently I have been exploring the [chainlink](http://chain.link) project and how
 I can utilize it in one of my personal projects.
One thing I wanted to see is how does the project scales, mainly I'm interested to
know how many concurrent [jobs](https://docs.chain.link/docs/jobs/) can be executed
with a limited resources and what are the profit margins of for a [chainlink node operator](https://docs.chain.link/docs/running-a-chainlink-node/).

This is my first attempt to layout some ground works on how you can run a load test
and I'll add more to this post as I go.

For now I have prepared a [Gatling load test](https://github.com/laocoon2525/chainlink-node-load-test)
which allows you to run a set jobs via curl commands (in reality these jobs will be
 triggered via smart contract or other means but for the sake of load testing we
  can trigger them via chainlink admin APIs)

You can follow in instruction under the [Gatling load test](https://github.com/laocoon2525/chainlink-node-load-test) in order to kick off a load test.

I'll update this post as I work my way on deploying chainlink on a cloud infrastructure
and then post some performance numbers explaining how does the pods and database
are performing under the load.

Stay tuned :)  
