---
title: How to find the container Id of a given process
---

<p class="lead">That sounded like a question for a test.</p>

Anyways if you use top and see a process that for any reason its not acting as expected,
CPU, memory, whatever, and all of your precautions in infrastructure are not handling it rigth,
you migth need to kill that container so that nomad, kubernetes or your Devops guy can
start a new container

You can do so with the PID of the process with this command

{% highlight bash %}

docker ps  | awk 'FNR>1 { print $1 }' | xargs docker inspect -f '{ {.State.Pid } }  { { .Id } } '  | grep $PID  | awk ' {print $2 }' | xargs docker stop

{% endhighlight %}
