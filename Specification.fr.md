# HONMA (HTTP Oriented Native Media Architecture)

## Introduction

HONMA est un protocole applicatif pour les interfaces de communication Web s'appuyant sur le protocole HTTP.

## Représentation des ressources

Les ressources sont représentées selon deux formes :

1. Collections : tableaux de ressources
2. Membres : ressources individuelles

En JSON, une collection est un tableau de hachages, un membre est un hachage. Chaque ressource doit inclure un identifiant unique dans son corps.

Le format de représentation est négocié via l'en-tête HTTP `Accept`.

## Navigation

Les relations entre ressources sont communiquées via l'en-tête HTTP `Link`. Chaque lien doit inclure au minimum :

- `rel` : type de relation
- `title` : libellé de la relation

L'attribut optionnel `id` peut être utilisé pour faire correspondre un lien à sa ressource associée dans la représentation.

## Métadonnées

Le verbe HTTP `OPTIONS` permet de découvrir la structure d'une ressource. Le format de représentation est négocié via l'en-tête HTTP `Accept`.

Chaque attribut de la ressource est décrit par les propriétés suivantes :

- `title` (type: `string`, nullifiable: `false`, default value `""`)
- `description` (type: `string`, nullifiable: `false`, default value `""`)
- `type` (type: `string`, nullifiable: `false`, default value `"string"`)
- `nullifiable` (type: `boolean`, nullifiable: `false`, default value `true`)
- `restricted_values` (type: `array`, nullifiable: `true`, default value `null`)
- `example` (nullifiable: `true`, default value `null`)

Si présent, chaque élément de `restricted_values` contient :
- `title` (type: `string`, nullifiable: `false`, default value: `""`)
- `description` (type: `string`, nullifiable: `false`, default value: `""`)
- `value` (nullifiable: `false`)

Si le format de représentation choisi ne permet pas d'exprimer certaines métadonnées, celles-ci doivent être omises de la réponse.

## Sécurité

En production, l'utilisation de HTTPS est fortement recommandée. L'URI étant le moyen d'accès à une ressource, il constitue naturellement sa clé d'accès.
