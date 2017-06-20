---
layout: post
title:  "Débuter un projet en 2017 : Supervision et alerting"
date:   2017-06-12 22:00:00
categories: fr
excerpt: Vous débutez un projet en 2017 ? Voici un ensemble d'article afin de partir du bon pied ! On commence avec la supervision et l'alerting
---

![Supervision]({{site.url}}/assets/alerting-supervision.png)

# Introduction

Cela fait maintenant plus de 2 ans que je suis sur mon projet : [Le Hub Numérique](https://www.hubnumerique.fr/) et j’y travaille depuis le début.
Durant cette période j’ai eu l’occasion de faire pas mal de bonnes choses et d’autres un peu moins bien.

Au cours des prochains articles je vais tenter de présenter de manière simple et concise comment démarrer un projet depuis zero en 2017. Nous allons parler principalement d’infrastructure, d’architecture et de développement.

Le but ici est d’avoir un petit ensemble d’articles pour débuter un projet dimensionnée pour une équipe de 5 à 35 personnes. Pour des équipes plus petites il n’y aura peut être pas besoin de tout ça. Pour les équipes plus grandes ça devrait tout aussi bien fonctionner (mais nous n’avons jamais dépassé les 35 sur mon projet donc à vos risques et périls!) les outils choisis sont scalables et bien sûr OpenSource.

Ces articles ne seront pas forcément très détaillées, mais pour certains sujets je ferais peut être des articles dédiés à un sujet précis !

Première étape : la supervision et l’alerting !

# Etape 0 : Supervision et alerting


> On ne peut améliorer que ce que l’on peut mesurer.


La supervision et l'alerting représente l’une des dernières choses que l’on a fait sur mon projet et rétrospectivement je pense qu’il aurait fallut le faire dès le début.

A tout moment au cours de la vie du projet nous avons eu besoin de mesurer des choses pour potentiellement les améliorer (ou se dire que tout allait bien et ne rien faire) et ne pas avoir d’outils pour cela nous a réellement desservi.

 Au niveau des logiciels à utiliser je pense qu’il est extrêmement intéressant d’utiliser la suite ELK (ElasticSearch, Logstash, Kibana) que ça soit pour les logs mais également pour les métriques systèmes. Cela simplifie énormément la montée en charge par rapport à un graphite (toi qui a essayé de scaler Graphite tu sais de quoi je parle!) et évite d’utiliser trop d’outils différents.

Du coup dans une première version je partirai sur les outils suivants:
- [_Elasticsearch_](https://www.elastic.co/fr/products/elasticsearch) : moteur de recherche
- [_Logstash_](https://www.elastic.co/fr/products/logstash) : traitement des logs (parsing et persistance dans ElasticSearch)
- [_Kibana_](https://www.elastic.co/fr/products/kibana) : recherche dans les logs et création de graphiques
- [_Elastalert_](https://github.com/Yelp/elastalert) : alerting

Parmi ces outils le seul un peu original est Elastalert développé principalement par Yelp. Son but est très simple, permettre de générer des alertes à partir d’une recherche ElasticSearch. Il existe de nombreux mécanismes d’alerting disponibles, on peut citer par exemple Slack, Mail, SMS,…
Bref un outil qui s’intègre à merveille dans la stack présentée ci-dessus et qui comble un vrai manque. Le bonus est que comme on gère également nos logs applicatifs dans ElasticSearch on peut alors recevoir des alertes à la fois sur des données techniques (mémoire, CPU, disques,…) mais également sur des paramètres métiers (un partenaire qui ne répond plus, une typologie d’erreurs particulière…).

Dans une deuxième version on ajoute à l’ensemble:

- [_Kafka_](https://kafka.apache.org/) : afin d’éviter que 50 logstash (1 par machine) envoie des données directement à un cluster ElasticSearch. Ajouter une zone tampon permet de parer aux coups durs et donne plus de flexibilité dans l’exploitation au quotidien.
- [_Grafana_](https://grafana.com/), pour les graphiques je trouve cela plus pratique et plus beau que Kibana même si celui-ci reste un must pour explorer vos données. Gère également un login/mot de passe ce que ne fait pas Kibana
- Collectd / Rsyslog / Beat / fluent… : car une JVM sur chaque machine pour logstash ça prend quand même un peu de mémoire. Il existe des tonnes d’outils permettant d’envoyer des données dans Kafka, choisissez votre préféré !

Ces outils représentent **UN** choix possible, il en existe bien sûr de nombreux autres (en particulier [Prometheus](https://prometheus.io/) dont on entend beaucoup parler récemment), testez, voyez ce qui marche le mieux pour vous et partagez !

# Conclusion

Avec ces outils vous êtes équipés pour gérer tous les logs et les métriques de votre projet pendant de nombreuses années, faites en bon usage !

Dans le prochain épisode nous parlerons d'intégration continue deuxième grande étape dans notre voyage !
