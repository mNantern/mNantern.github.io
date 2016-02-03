---
layout: post
title:  "Puppet, RedHat et le dossier limits.d"
date:   2014-01-27 23:00:00
categories: fr
excerpt: Cette semaine j'ai voulu installer Oracle 11g avec Puppet sur RedHat en utilisant le dossier limits.d
---
![Puppet]({{site.url}}/assets/PL_logo.png)

Cette semaine j'ai voulu automatiser l'installation d'Oracle 11g via Puppet afin de simplifier un peu l'opération. L'une des étapes consiste à mettre en place des limites pour l'utilisateur Oracle comme indiqué sur [cette page RedHat](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Tuning_and_Optimizing_Red_Hat_Enterprise_Linux_for_Oracle_9i_and_10g_Databases/chap-Oracle_9i_and_10g_Tuning_Guide-Setting_Shell_Limits_for_the_Oracle_User.html).

Et pour une fois que RedHat dispose d'un dossier tel que "limits.d" autant s'en servir afin de placer directement notre fichier dans ce dossier /etc/security/limits.d via Puppet. Le fichier ressemble à ça:
{% highlight bash %}
oracle           hard    nofile          65536
{% endhighlight %}

La valeur par défaut étant de 1024.

L'étape suivante consistait à lancer l'installeur Oracle avec l'utilisateur oracle qui devait donc prendre en compte les paramètres mis dans ce fichier. Et là c'est le drame ! L'installeur Oracle se plaint que les paramètres "security" ne sont pas appliqués...

Afin de mieux comprendre le problème je fais un petit test via Puppet:

{% highlight puppet %}
exec{'test ulimit user': 
  command => "/bin/su - oracle -c 'ulimit -Hn' > /tmp/logMN", 
}

exec{'test ulimit user pup': 
  command => "/bin/bash -c 'ulimit -Hn' > /tmp/logMN2", 
  user => "oracle", 
}
{% endhighlight %}

Le premier exec teste le nombre maximum de descripteurs de fichiers (limite "hard") via un changement d'utilisateur classique via su. Le second test execute la même commande, également avec l'utilisateur "oracle", mais via Puppet et le paramètre "user".

Et les résultats:
{% highlight bash %}
[root@lxpt106 ~]# more /tmp/logMN* 
:::::::::::::: /tmp/logMN :::::::::::::: 
65536 
:::::::::::::: /tmp/logMN2 :::::::::::::: 
1024
{% endhighlight %}

Mes craintes se concrétisent, un exec Puppet avec le paramètre "user" ne se comporte pas comme un su classique.Visiblement Puppet ne prend pas en compte PAM lors du changement d'utilisateur. Peut-être faut-il configurer quelque chose de particulier dans PAM.
En attendant je pars sur la solution 1 avec un su à l'ancienne.

J'ai également trouvé un bug ouvert sur le [bug tracker Puppet](http://projects.puppetlabs.com/issues/4707) mais visiblement il n'est pas envisagé de corriger ce fonctionnement.

En attendant la solution 1 fonctionne bien !
