# HONMA (HTTP Oriented Native Media Architecture)

Version: 1.0-draft
Status: Draft

## Introduction

HONMA est un protocole applicatif pour les interfaces de communication Web s'appuyant sur HTTP et EWLP.

## Ressources

Une ressource est accessible via une ou plusieurs URI et existe sous deux formes :
1. Collection : ensemble de ressources d'une même classe
2. Membre : ressource individuelle

La représentation d'une ressource est négociée via l'en-tête `Accept`.

## Format JSON

Une collection est représentée par un tableau de hachages, un membre par un hachage.

Chaque ressource membre doit inclure :
- `id` : identifiant unique dans sa classe
- `uri` : URI canonique

## Navigation

Les relations entre ressources sont exprimées via l'en-tête `Link`. Conformément à EWLP, chaque lien doit inclure :
- URI de référence
- `rel` : type de relation
- `title` : libellé
- `class` : classe de la ressource cible
- `id` : identifiant (pour les membres uniquement)

## Métadonnées

Le verbe `OPTIONS` expose la structure d'une ressource :
- Description des attributs
- Relations possibles
- Opérations autorisées (via `Allow`)

## Références

- RFC 7231 (HTTP/1.1)
- RFC 8288 (Web Linking)
- EWLP 1.0
