---
layout: post
title:  "Gatling & Authentication (Dilemma)"
date:   2022-06-17 10:54:41 -0700
categories: Gatling Load Testing
---
As I was configuring  the load testing project that I was working on I ran into an issue:
* How can I authenticate to the server that I'm load testing and share the token
 for all scenarios?

In the past I had written a script which did the authentication and set the token
 as an environment variable, this approach works fine but what if I want everything
 to be done in one go? like the authentication will also be part of the process rather
 than a separated step.  

Turns out you can simply do that by adding a gradle rule to your `build.gradle` file.
In the example below I assume you are testing your code against a server running
on localhost and the authentication code accepts a request with body payload that
includes the username & password and sets a session cookie which includes the token.
In this code I read that cookie and set it as a java property `___sessionToken`
 to be used by the application later.

{% highlight gradle %}
tasks.addRule('Set variables for the test to run') { String taskName ->
    if(taskName.startsWith('gatlingRun')) {

        def req = new URL('http://localhost/sessions').openConnection()
        req.setRequestMethod("POST")
        req.setRequestProperty("Content-Type", "application/json; charset=UTF-8")
        req.setDoOutput(true)
        req.getOutputStream().write(('{"email":"'+System.env.ADMIN_USER+'","password":"'+System.env.ADMIN_PASSWORD+'"}').getBytes())
        System.setProperty("___sessionToken", req.getHeaderField('Set-Cookie'))
    }
}
{% endhighlight %}

And here is how you can access the token in the code

{% highlight scala %}

val sessionToken = System.getProperty("___sessionToken")

setUp(
    scenario("Execute Jobs")
      .exec(
        http("execute job")
          .post("http://localhost/<test-end-point>")
          .header("Cookie", sessionToken)
      ).inject(atOnceUsers(1))
      .protocols(<<httpProtocol>>)
  )

{% endhighlight %}

in order to run this code you can do something as follow. Note that this configuration
is compatible with gatling gradle plugin for running both
* all scenarios
 or
* an individual one.

{% highlight bash %}
export ADMIN_USER='<username>'
export ADMIN_PASSWORD='<password>'

./gradlew gatlingRun

OR

./gradlew gatlingRun-<Your fully qualified scenario class>

{% endhighlight %}

A working copy of this can be found [here](https://github.com/laocoon2525/chainlink-node-load-test/blob/main/build.gradle)

Happy perf testing :)
