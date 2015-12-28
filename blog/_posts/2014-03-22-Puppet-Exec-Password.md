---
layout: post
title:  "Dealing with password in Puppet Exec"
date:   2014-03-22 12:00:00
categories: en
excerpt: Sometimes you need to use password in a Puppet exec. But you don't want it to be displayed in case of an error.
---
![Puppet_logo]({{site.url}}/assets/PL_logo.png)


The issue is simple: I don't want that my password appear in Puppet log if the command fail. But you still have to pass it in order to make that Exec ! Here is how to dow it:

First the exec:

{% highlight ruby %}
exec{'my_awesome_exec':
  command => '/home/matt/swag.sh -pass=fooBar'
}
{% endhighlight %}

And the resulting output:


    Error: /home/matt/swag.sh -pass=fooBar returned 1 instead of one of [0]
    Error: /Stage[main]/Ntp/Exec[my_awesome_exec]/returns: change from notrun to 0 failed: /home/matt/swag.sh -pass=fooBar returned 1 instead of one of [0]

The best solution I found to deal with that is to use environment variable like that:

{% highlight ruby %}
exec{'my_awesome_exec':
  command     => '/home/matt/swag.sh -pass=$PASS',
  environment => 'PASS=fooBar'
}
{% endhighlight %}

Be careful with the simple quote around the "command". If you use classic quote ("), Puppet will try to replace $PASS so the resulting command will be:

    /home/matt/swag.sh -pass=

Not what we want.

And the final result:

    Error: /home/matt/swag.sh -pass=$PASS returned 1 instead of one of [0]
    Error: /Stage[main]/Ntp/Exec[my_awesome_exec]/returns: change from notrun to 0 failed: /home/matt/swag.sh -pass=$PASS returned 1 instead of one of [0]

Much better !
