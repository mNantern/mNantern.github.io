---
layout: post
title:  "Débuter un projet en 2017 : Intégration continue"
date:   2017-06-22 17:00:00
categories: fr
excerpt: Vous débutez un projet en 2017 ? Voici un ensemble d'article afin de partir du bon pied ! On continue avec la gestion de code source et l'intégration continue
---

![Supervision]({{site.url}}/assets/Integration_continue.png)

# Introduction

Après un première article sur la [supervision et l'alerting](https://blog.nantern.com/fr/2017/06/12/Projet-Supervision-Alerting.html) on continue notre chemin pour poser des bases solides à la construction d'un projet en 2017.

Aujourd'hui la gestion de sources et l'intégration continue !

# Etape 1 : Intégration continue

> Tout, absolument tout doit être dans votre gestionnaire de versions.

La règle ci-dessus est extrêmement importante,  et nous avons fait cette erreur avec notre instance Jenkins qui s’est progressivement transformée en dépôt de code source alternatif pour les scripts de tests, d’exploitation et de déploiement.

Quand on en vient à devoir sauvegarder la configuration de Jenkins car il y a bien trop de code uniquement présent dans celui-ci c’est le signe d’un vrai problème.

Suite à ce constat nous avons décidé de remplacer presque intégralement Jenkins par Gitlab CI. Le point positif est la très bonne intégration entre la partie gestion de sources et la partie intégration continue (encore heureux ! 😁), plus besoin de gérer des hooks entre Gitlab et Jenkins.
Autre point primordial, il n’y a aucune IHM (ça se dit ça encore en 2017 ?) dans Gitlab CI, tout doit être dans le code source, défini sous forme d’un pipeline (et c'est pas plus mal 🙃).

Pour le déploiement des *runners* (les machines exécutant les jobs d'intégration continue) on va utiliser Docker. Cela permet de réduire l’installation de la machine à sa plus simple expression: Docker et c’est tout !

Plus besoin de mettre à jour tous ses runners quand on ajoute une nouvelle brique logicielle (NodeJs, Go, ...), il suffit uniquement d'utiliser l’image Docker dédiée et c’est bon (en plus cela servira aussi pour le développement local).

Vos runners éviteront ainsi de se transformer en machine fourre-tout avec 3 versions de Java, 7 de NodeJs et un phantomJs dans un coin que tout le monde à oublié.

Au niveau des outils utilisés donc:
- [_Git_](https://git-scm.com/) : incontournable, on ne le présente plus
- [_Gitlab_](https://about.gitlab.com/) : un gestionnaire de code source et bien bien plus
- [_Docker_](http://docker.com/) : pour conteneuriser vos applications

Trucs et astuces :
- Prévoir dès le début une sauvegarde externalisée de git et la tester
- Ce qui doit être dans votre dépôt git : le code bien sûr, les tests, l’infrastructure (on en reparle dans la partie suivante), la documentation de votre projet. Vous faîtes un petit script ? Ajoutez le à Git ça pourra toujours servir à nouveau
- Ce qui ne doit pas être dans votre dépôt git : les binaires, les grosses maquettes d’écran, bref tout ce qui fait plus de 1Mo et bien sûr les mots de passes et secrets divers.
- Au niveau des métriques intéressantes à suivre il y a le temps des différentes étapes de votre pipeline, le nombre d’exécutions en cours et le nombre d’exécutions en attente.
- N’oubliez pas d’ajouter une alerte en cas d’échec de la sauvegarde de votre dépôt git ou d’un nombre trop important de jobs en attente

# Conclusion

Avec ça nous disposons d’une base solide pour gérer le code source de notre projet et toute la partie intégration continue.
Nous allons ensuite passer à l’étape suivante à savoir construire notre premier environnement pour déployer notre code source et c’est ce que nous verrons dans la troisième partie.
