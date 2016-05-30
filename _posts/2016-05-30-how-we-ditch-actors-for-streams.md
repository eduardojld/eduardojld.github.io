---
title: How we ditch actors for streams
---

<p class="lead">Scala its awesome, Akka actors are awesome-er, Akka Streams are next level awesome, they are like deadpool awesome</p>


Ok, so I love akka actor based solutions, I do. You probably do too. But, if you, like us have tried to do
heavy lifting with actors, or have played around with the backpressure concepts, or have tried to do clustering with actors to gain performance, and ... I could go on and on, but if you have, you know or will soon enough that its not such an easy task as a "normal" actor system

And thats ok. I think.

So what does akka streams gives you, so that things get to be as promised. that is:

 1.- Simple!

 2.- Fast!

And those are pretty much everyones concern. So what is thats so awesome about akka streams.

You can start looking at your solutions as a graph, not in a abstract way, but an actual graph, yeah really! you can pretty much draw your solution, let start learning some concepts

To start working in akka streams, everyone needs to talk the same language, for that to happen you need to know basically three things

1.- Source
    Its the start node of every graph, 0 inputs, 1 output
2.- Sink
    Its the end node of every graph, 1 input, 0 output
3.- Flow
    Are all the nodes in between, 1 input, N output

So lets say you want to build a never ending program that sends some data from point A to point B

You would normally start something that reads in a infinite loop and inside that loop you would call some methods or have all your code just stuffed in there, prone to error, or giant try/catch blocks

 The way youll do that in akka streams means thinking in a pipe where items flow as single units, from point of origin A (if you are thinking source,  you are rigth) to some aggregation or transformation node, and then to destination B (you got it! a Sink), so this would look like in actual code

{% highlight scala %}

 SourceA  ~>  NodeTransformation  ~> SinkB  

{% endhighlight %}


So need to gain performance, no worries, akka streams has a ton of stages (http://doc.akka.io/docs/akka/2.4.3/scala/stream/stages-overview.html#grouped) that help you seemeasly with that, such as Flow.mapAsync or Sink.foreachParallel. So, lets imagine that both A and B are API we are using, you could do something like

{% highlight scala %}

 SourceA  ~>  NodeTransformation  ~> Sink.foreachParallel(100){ hitAPI( _ ) }

{% endhighlight %}

In the above example you would be hitting the API with 100 workers in parallel just like that! boom! boost of performance.


Take a look at this post for more info
http://eng.localytics.com/akka-streams-akka-without-the-actors/
