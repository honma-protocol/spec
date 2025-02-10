# Spécification EWLP 2.0 (Extended Web Linking Protocol)

## 1. Introduction

EWLP étend le protocole Web Linking (RFC 8288) en ajoutant les attributs optionnels `class` et `id` aux liens. Ces attributs permettent respectivement de catégoriser les ressources et de les identifier de manière stable.

## 2. Définitions

### Ressource

Une entité accessible via une ou plusieurs URIs.

### Catégorie

La catégorie d'une ressource est définie par son attribut `class`. Une ressource peut appartenir à plusieurs catégories.

### Identité

L'identité d'une ressource est définie selon les règles suivantes :
1. Si `class` et `id` sont présents : la combinaison {`class`, `id`}
2. Si seul `id` est présent : la valeur de `id`
3. Si `class` et `id` sont absents : l'URI de la ressource

## 3. Contraintes

1. L'attribut `class`, s'il est présent, DOIT suivre les règles suivantes :
   - Une ou plusieurs classes séparées par des espaces
   - Chaque classe doit être un token valide selon les règles HTML
   - Les espaces en début et fin sont ignorés
   - Les espaces multiples sont normalisés en un seul espace

2. L'attribut `id`, s'il est présent, DOIT suivre les règles suivantes :
   - Un seul token valide selon les règles HTML
   - Les espaces en début et fin sont ignorés

3. Deux liens avec la même identité DOIVENT représenter la même ressource

## 4. Exemples

### 4.1 Même ressource (même classe et id)

```http
Link: </articles/42>; rel="canonical"; class="article"; id="42"; title="Mon Article",
      </feed/1>; rel="alternate"; class="article"; id="42"; title="Mon Article"
```

### 4.2 Ressources différentes (même id, classes différentes)

```http
Link: </articles/42>; rel="item"; class="article"; id="42"; title="Article #42",
      </comments/42>; rel="item"; class="comment"; id="42"; title="Commentaire #42"
```

### 4.3 Ressources de même catégorie

```http
Link: </articles/42>; rel="item"; class="article"; id="42"; title="Premier Article",
      </articles/57>; rel="item"; class="article"; id="57"; title="Second Article"
```

### 4.4 Ressource identifiée uniquement par id

```http
Link: </about>; rel="help"; id="about"; title="Documentation"
```

### 4.5 Ressource identifiée par URI

```http
Link: </search?q=test>; rel="search"; title="Résultats de recherche"
```

## 5. Exemple complet d'interaction

### Requête

```http
GET /feed HTTP/1.1
Accept: application/json
```

### Réponse

```http
HTTP/1.1 200 OK
Content-Type: application/json
Link: </feed>; rel="self"; class="feed"; title="Fil d'actualité",
      </articles/42>; rel="item"; class="article"; id="42"; title="Article #42",
      </comments/42>; rel="item"; class="comment"; id="42"; title="Commentaire #42",
      </about>; rel="help"; id="about"; title="Documentation"

[
  {
    "class": "article",
    "id": "42",
    "title": "Mon article"
  },
  {
    "class": "comment",
    "id": "42",
    "content": "Un commentaire"
  }
]
```
