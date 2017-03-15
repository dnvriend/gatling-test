# gatling-test
A small study project on [gatling](http://gatling.io/#/), an open-source load testing framework based on Scala,
Akka and Netty that features High performance, Ready-to-present HTML reports and Scenario recorder and developer-friendly DSL.

## Introduction
Gatling is a highly capable load testing tool. It is designed for ease of use, maintainability and high performance.

Out of the box, Gatling comes with excellent support of the [HTTP](http://gatling.io/docs/2.2.1/http/index.html)
and [JMS](http://gatling.io/docs/2.2.1/jms.html) protocols. The core engine is protocol agnostic, it is perfectly
possible to implement support for other protocols, the source code is available on [github](https://github.com/gatling/gatling).

## Howto
Create a .scala file in the 'test' project like the following:

```scala
package com.github.dnvriend.simulations

import io.gatling.core.Predef._
import io.gatling.http.Predef._
import scala.concurrent.duration._

class GoogleSimulation extends Simulation {
  val httpConf = http
    .baseURL("https://www.google.nl") // The baseURL that will be prepended to all relative urls.
    .acceptHeader("text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8") // Common HTTP headers that will be sent with all the requests.
    .doNotTrackHeader("1")
    .acceptLanguageHeader("en-US,en;q=0.5")
    .acceptEncodingHeader("gzip, deflate")
    .userAgentHeader("Mozilla/5.0 (Windows NT 5.1; rv:31.0) Gecko/20100101 Firefox/31.0")

  val scn = scenario("BasicSimulation") // The scenario definition.
    .exec(http("request_1") // A HTTP request, named request_1. This name will be displayed in the final reports.
      .get("/")) // The url this request targets with the GET method.
    .pause(5) // Some pause/think time.

  setUp( // Where you set up the scenarios that will be launched in this Simulation.
    scn.inject(atOnceUsers(1)) // Declaring to inject into scenario named scn one single user.
  ).protocols(httpConf) // Attaching the HTTP configuration declared above.
}
```

and launch the test with `sbt gatling:test` or `sbt gatling:testOnly *GoogleSimulation`

## Reporting
To generate a report run `sbt gatling:generateReport`. You find the generated reports in `target/gatling/<simulation-name>`.

## Resources
- [Gatling Quickstart](http://gatling.io/docs/2.2/quickstart/)
- [Gatling Advanced Tutorial](http://gatling.io/docs/2.2/advanced_tutorial/#advanced-tutorial)

## Video
- [(0'29 hr) Blast your webapp with Gatling - Romain Sertelon](https://www.youtube.com/watch?v=pIGo77gX2bs)
- [(0'58 hr) Blast Your App With Gatling - Swapnil Kotwal](https://www.youtube.com/watch?v=Y3XQCBUWgDE)
- [(0'44 hr) Stresstests with Gatling - Niko Köbler](https://www.youtube.com/watch?v=gOZvtBYzIVc)
- [Video series - Performance Testing with Gatling](https://www.youtube.com/watch?v=fqP6PTUdtkY&list=PLd4gvNaNZ4T3NCWsv3zwHYlLGtr9s1-Fz)

Have fun!