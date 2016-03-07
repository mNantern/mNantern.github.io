---
layout: post
title:  "Sites statiques, noms de domaines et SSL"
date:   2016-03-07 22:00:00
categories: fr
excerpt: Très souvent il est possible d'héberger gratuitement ses contenus statiques (html, css, js, ...) avec son propre nom de domaine. Mais bien souvent l'offre s'arrête là et il n'est pas possible d'ajouter en plus du https avec les offres gratuites. Nous allons voir dans cet article comment faire ça (et gratuitement en plus !)
---

![Cloudflare]({{site.url}}/assets/cloudflare.jpg)

Très souvent il est possible d'héberger gratuitement ses contenus statiques (html, css, js, ...) avec son propre nom de domaine. Mais bien souvent l'offre s'arrête là et il n'est pas possible d'ajouter en plus du https avec les offres gratuites. Nous allons voir dans cet article comment faire ça (et gratuitement en plus !)

## Héberger ses contenus statiques

Actuellement il existe de nombreux sites permettant d'héberger des contenus statiques gratuitement. Par exemple on peut citer [GitHub Pages](https://pages.github.com/) qui abrite actuellement [le blog](https://blog.nantern.com/fr/) ou [surge.sh](https://surge.sh/) qui héberge [la page d'accueil](https://nantern.com) (oui j'aime bien tester :wink:).

Ces solutions sont très pratiques et permettent de ne pas avoir à gérer soi-même l'hébergement des contenus tout en permettant d'en changer rapidement au cas où (tout est dans un dépôt git après tout). En plus avec ces solutions on bénéficie des CDN, du réseau et des serveurs de grandes sociétés.

On peut même configurer son propre nom de domaine si on en a assez des urls en `github.com` ou en `surge.sh`.

Seulement on est quand même en 2016 et il faut du HTTPS. Partout. Et c'est bien souvent là que ces solutions montrent leurs limites: pour surge.sh il faut prendre l'offre payante et pour github rien ne semble prévu.

Mais il existe des solutions !

## Ajouter le SSL

La plus connue est d'utiliser [Cloudflare](https://www.cloudflare.com) qui est un CDN mais qui offre aussi dans son offre gratuite le support du SSL (et [d'autres trucs](https://www.cloudflare.com/plans/)).

Et c'est ultra-facile à faire:
1. S'inscrire sur Cloudflare
2. Entrer l'adresse de son site
3. Modifier les nameservers de votre registrar (le site sur lequel vous avez achetez votre certificat) pour pointer vers ceux fournis par Cloudflare
4. Dans **Crypto > SSL** passer le paramètre à la valeur **Flexible**

Et voilà !

## Limitation et aller plus loin

:warning: Passer le mode à Flexible implique que la connection entre les clients et Cloudflare sera en HTTPS mais celle entre Cloudflare et GitHub Pages sera en HTTP. Mais bon on a pas trop le choix...

Par contre ce qu'il est possible de faire toujours avec l'offre gratuite de Cloudflare c'est de mettre en place des règles qui forcent le https pour les clients (3 règles sont offertes par site).
Pour cela il suffit d'aller dans la page **Page Rules**, de créer une nouvelle règle et d'activer *Always use https*.

Il existe également d'autres sites qui font ça comme par exemple [KloudSec](https://kloudsec.com/github-pages/new) mais je n'ai jamais testé.

C'est tout pour aujourd'hui.

Stay tuned and See ya !
