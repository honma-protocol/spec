# RINKEI (輪系) - Resource Identification & Naming for Knowledge Exchange & Interoperability

Version: 1.0-draft
Status: Draft

## Abstract

Cette spécification étend le modèle Web Linking défini dans la RFC 8288 en introduisant de nouvelles extensions de liens pour fournir une identification canonique des ressources web. Ce protocole permet l'identification unique des ressources à travers différentes URI et facilite la classification des types de ressources.

## 1. Introduction

Dans les architectures REST, une même ressource peut être identifiée par plusieurs URI. Bien que cette flexibilité soit précieuse, elle peut créer une ambiguïté lors de la détermination de l'équivalence entre deux URI. RINKEI introduit une méthode standardisée pour fournir une identification canonique des ressources via les en-têtes HTTP Link.

## 2. Terminologie

Les mots clés "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY" et "OPTIONAL" dans ce document doivent être interprétés comme décrit dans la RFC 2119.

- Ressource : Telle que définie dans la RFC 7231
- Lien : Tel que défini dans la RFC 8288
- Collection : Une ressource représentant un ensemble d'autres ressources
- Membre : Une ressource individuelle pouvant appartenir à une ou plusieurs collections

## 3. Extensions de liens

Cette spécification définit deux nouvelles extensions de liens :

### 3.1. class

L'extension "class" indique le type de la ressource ciblée. Sa valeur MUST être une chaîne qui identifie la classe de ressource.

- REQUIRED pour tous les liens
- MUST être unique dans le contexte d'une API
- MUST contenir uniquement des caractères alphanumériques, des tirets et des underscores
- SHOULD être en minuscules
- SHOULD être au pluriel pour les collections (ex: "articles" plutôt que "article")

### 3.2. id

L'extension "id" fournit un identifiant unique pour une ressource membre au sein de sa classe.

- REQUIRED pour les liens vers des ressources membres
- OPTIONAL pour les liens vers des ressources collections (SHOULD NOT être utilisé)
- MUST être unique dans le périmètre de la classe de ressource
- MAY contenir tout caractère autorisé dans les segments de chemin URL

## 4. Utilisation

### 4.1. Ressources membres

Pour les liens vers une ressource membre, `class` et `id` MUST être fournis :

```http
Link: </articles/42>; rel="canonical"; class="articles"; id="42"
```

### 4.2. Ressources collections

Pour les liens vers une ressource collection, seul `class` MUST être fourni :

```http
Link: </articles>; rel="collection"; class="articles"
```

### 4.3. Identité des ressources

Deux ressources MUST être considérées comme identiques si elles partagent à la fois :
1. La même valeur `class`
2. La même valeur `id`

### 4.4. Liens canoniques

Il est RECOMMENDED d'inclure un lien avec `rel="canonical"` lorsqu'une ressource est accessible via plusieurs URI :

```http
Link: </articles/42>; rel="canonical"; class="articles"; id="42"
```

## 5. Exemples

### 5.1. Ressource collection

```http
GET /articles HTTP/1.1
Host: example.com

HTTP/1.1 200 OK
Link: </articles>; rel="self"; class="articles"
Link: </articles?page=2>; rel="next"; class="articles"
Link: </articles/42>; rel="item"; class="articles"; id="42"
Link: </articles/43>; rel="item"; class="articles"; id="43"
```

### 5.2. Ressource membre avec points d'accès multiples

```http
GET /comments/827/article HTTP/1.1
Host: example.com

HTTP/1.1 200 OK
Link: </articles/42>; rel="canonical"; class="articles"; id="42"
Link: </comments/827>; rel="up"; class="comments"; id="827"
```

## 6. Considérations de sécurité

Ce protocole n'introduit pas de nouvelles considérations de sécurité au-delà de celles discutées dans la RFC 8288. Cependant, les implémenteurs doivent être conscients que l'exposition d'identifiants internes pourrait fournir des informations sur la structure sous-jacente du système.

## 7. Références

### 7.1. Références normatives

- [RFC 2119] Mots clés pour une utilisation dans les RFC
- [RFC 7231] Hypertext Transfer Protocol (HTTP/1.1): Sémantique et Contenu
- [RFC 8288] Web Linking

### 7.2. Références informatives

- [RFC 3986] Uniform Resource Identifier (URI): Syntaxe Générique
