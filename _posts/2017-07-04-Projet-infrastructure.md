---
layout: post
title:  "Débuter un projet en 2017 : Infrastructure"
date:   2017-07-04 17:00:00
categories: fr
excerpt: Vous débutez un projet en 2017 ? Voici un ensemble d'article afin de partir du bon pied ! On poursuit avec la gestion de votre infrastructure
---

![Infrastructure]({{site.url}}/assets/Infrastructure.png)

Vous avez maintenant de quoi [monitorer vos plateformes](https://blog.nantern.com/fr/2017/06/12/Projet-Supervision-Alerting.html), [stocker votre code source](https://blog.nantern.com/fr/2017/06/22/Projet-Integration-Continue.html) et construire vos livrables. Il ne reste plus qu’à avoir une (ou des) plateformes pour déployer vos petits livrables fait avec ❤️.

Pour cela une règle simple:

> Rien ne doit être fait à la main en ce qui concerne l’infrastructure, cela servira à nouveau plus tard.

Que vous soyez sur un petit ou un gros projet tout le code et les scripts nécessaires à la construction d’une plateforme seront à nouveau utilisés plus tard que ça soit pour créer une nouvelle plateforme ou reconstruire la plateforme existante.

De plus les configurations réalisées à la main sur les plateformes sont une source inépuisable d’anomalies qui vous prendront de nombreuses heures (cette petite règle iptables perdue lors du reboot de la machine hante encore mes nuits 😭). Donc prenez un peu plus de temps mais faîtes les choses bien, votre futur vous vous remerciera !

Au niveau de l’outillage nécessaire on en distingue trois différents:
1. Un premier pour décrire votre infrastructure (combien de VM, combien de CPU, de RAM, quel réseau etc.). Pour ce besoin l’outil de référence est [Terraform par HashiCorp](https://www.terraform.io/) qui possède un grand nombre de plugin pour tous les providers (AWS, GCE, Azure, VSphere,…)
2. Si la VM décrite dans Terraform n’existe pas alors celui-ci va la créer et pour créer cette VM un template sera nécessaire. Afin de créer facilement ce template un outil tel [Packer](https://www.packer.io/) (toujours chez HashiCorp) fonctionnera à merveille
3. Une fois la VM instancié il faudra déployer vos logiciels dessus (un JDK, Tomcat, NodeJs,…), installer votre application et configurer le tout. Pour cela partez sur un outil d’infrastructure as Code à la [Puppet](https://puppet.com/) ou [Ansible](https://www.ansible.com/). Prenez celui que vous connaissez le mieux, il fera parfaitement l’affaire
4. Il reste encore la question de « comment livrer mon code source » ? De notre côté nous sommes partis sur le système de package de notre OS (RPM en l’occurence) car c’est une solution standard, éprouvée, qui fonctionne peu importe le langage utilisé et qui est très bien supporté par Ansible ou Puppet

Ou si vous êtes du type aventurier vous pouvez remplacer tout cela par le couple Kubernetes / Docker:
- [Kubernetes](https://kubernetes.io/) : pour réaliser tout ce qui est orchestration de conteneurs (je veux que mon conteneur soit déployé en 3 exemplaires minimum mais s’il y a de la charge tu peux monter jusqu’à 10 par exemple)
- [Docker](https://www.docker.com/) : pour la partie conteneurisation

Bien entendu cette solution vient également avec ses challenges comme la supervision des conteneurs, la centralisation des logs, le service discovery.
Bref peu importe la solution choisie il y aura du travail !

Trucs et astuces:
- Ces outils fonctionnent tout aussi bien si vous devez utiliser un cloud ou une solution interne (à la VSphere)
- La partie Packer n’est pas forcément obligatoire si vous utilisez une seule image de base customisée ensuite par Puppet ou Ansible. Sur mon projet nous avons une seule image CentOs avec le client puppet et c’est tout !
- Puppet dans la version OpenSource ne permet pas de faire de l’orchestration (installer d’abord la machine A, puis accepter un échange de clé sur la machine B et finir par le redémarrage d’un service sur la machine A par exemple). Dans ce cas Ansible peut très bien faire l’affaire (même pour lancer uniquement Puppet dans un certain ordre sur certaines machines).
- Au niveau des métriques intéressantes à suivre il y a:
  - La durée d’un déploiement complet de votre plateforme
  - Les métriques de base de vos VM/conteneurs (CPU, RAM, disque, réseau,…)
- En ce qui concerne l’alerting il faut que chaque machine de votre plateforme soient accompagnées d’un ensemble d’alerte sur les métriques de base:
  - Pourcentage d’occupation de vos disques (extrêmement utile 👌)
  - Utilisation de la mémoire, de la swap, du CPU
  - Si une machine ne remonte aucun log pendant une durée définie c’est en général le signe que quelque chose ne va pas bien
  - N’hésitez pas à en ajouter au fur et à mesure de vos problèmes

A l'issue de cet article nous avons maintenant une plateforme complète : entièrement automatisée, supervisée et dont tout le code source est disponible dans un dépôt sauvegardé. On peut enfin passer à d'autres sujets !
Voici ce qui arrive prochainement :
- Comment organiser son développement d'un point de vue applicatif ?
- Comment consommer une API Rest ?
- Comment exposer une API Rest ?

Stay tuned and See ya !
