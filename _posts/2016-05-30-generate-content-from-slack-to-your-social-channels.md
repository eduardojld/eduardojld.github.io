---
title: Your company slacks channels are awesome, but your social networks are dead
---

<p class="lead">We know how that feels</p>

We were generating tons of content in slack, but our social networks were not seeing any activity for weeks, who am i kidding, months.

We needed to take advantage of that content that was so cool and try to share it outside our slack channels.... to the world! (I needed to put that, it just felt rigth)

Thats when the [scala-slack-bot](https://github.com/ScalaConsultants/scala-slack-bot) libraries come into play.

We started playing with the examples and everything they provide that allow us to, based on slack reactions, post interesting content back to the companies twitter feed (soon to facebook and linkedin).



###How does it look

The scalac library provides mostly everything you need, the only tweak we neeeded to do was to handle reactions, which are not received in your actors as it is, so you need to add a snippet to your receive method, kind of like this

{% highlight scala %}

case UndefinedMessage(text) if( text.size > 0 ) =>
  val mType = text.parseJson.convertTo[MessageType]
  val incomingM :Option[Reaction] = mType match {
      case MessageType("reaction_added", _ ) =>
          Some(text.parseJson.convertTo[Reaction])
      case _ =>
          log.info("nothing")
          None
    }

{% endhighlight %}

That way you can handle reactions as regular messages are handled within your actor, and then take actions based on those reactions.

Other than scalac lib, we are relying on twitter4j and spray support for jsons
