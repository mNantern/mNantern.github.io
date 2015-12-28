---
layout: post
title:  "Puppet: Parameter name failed on Package"
date:   2014-03-22 22:00:00
categories: en
excerpt: When I upgraded to Puppet 3.4, I got the following error "Parameter name failed on Package Name must be a String not Array"
---
![Puppet_logo]({{site.url}}/assets/PL_logo.png)


Since Puppet 3.4 (or Puppet Enterprise 3.2) a useful trick does not work anymore.
It was an undocumented feature that allowed you to declare an alias on a list of packages in order to facilitate dependency handling.

For example if you want that an Exec requires a list of packages, you could write:

{% highlight ruby %}

$packages_list = ['pkg1','pkg2','pkg3']

package{'alias':
  ensure => installed,
  name   => $packages_list,
}

exec{'my_awesome_exec':
  command => '...'
  require => Package['alias']
}
{% endhighlight %}

But that is not working anymore, you get the following error now:

    Parameter name failed on Package['alias']: Name must be a String not Array

You can still keep the old behavior by using the "before" parameter:

{% highlight ruby %}

package{$packages_list:
  ensure => installed,
  before => Exec['my_awesome_exec'],
}

exec{'my_awesome_exec':
  command => '...'
}
{% endhighlight %}

I never think of this (meta-)parameter...
