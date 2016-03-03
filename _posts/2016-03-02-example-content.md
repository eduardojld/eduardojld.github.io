---
title: A bot for Slack using Akka
---

<p class="lead">Sure Im not the only one who wanted to find a Slack integration that does something and cant find a bot that out of box handles our crazyness.</p>

So, how about you build one of your own?

I wanted to build a bot that we could use to track work hours, trying to find stuff I bumped into Scalac's [scala-slack-bot](https://github.com/ScalaConsultants/scala-slack-bot) a couple of weeks ago and took it out for a spin, and its fantastic!!

###How does it look

A simple actor becomes a bot in no time with this lib

{% highlight scala %}

class HelloBot(override val bus: MessageEventBus) extends AbstractBot {
  val welcomes = List("what's up?",
                      "how's going?",
                      "ready for work?",
                      "nice to see you")

  def welcome = Random.shuffle(welcomes).head

  override def act: Receive = {
    case Command("hello", _ , message) =>
            publish(OutboundMessage(message.channel,
                    s"hello <@${message.user}>,\\n $welcome"))
    case _ => \\\\nothing to do
    }


  override def help(channel: String): OutboundMessage =
          OutboundMessage(channel,
                "When you feel lonely and unwanted" +
                s"*$name* is something for you. \\n " +
                "`hello` - to talk with the bot")
}

{% endhighlight %}

Super usefull to get stuff done in no time!
