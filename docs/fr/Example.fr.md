# HONMA - Guide par l'exemple

## Ressources et représentations

### Collection d'articles

```http
GET /articles
Accept: application/json
```

```json
[
  {
    "id": "42",
    "uri": "/articles/42",
    "titre": "Introduction à HONMA",
    "date_publication": "2024-02-01",
    "statut": "publie"
  },
  {
    "id": "45",
    "uri": "/articles/45",
    "titre": "Architecture REST",
    "date_publication": null,
    "statut": "brouillon"
  }
]
```

```http
Link: </articles>; rel="self"; class="articles"; title="Articles"
Link: </articles/42>; rel="item"; class="articles"; id="42"; title="Introduction à HONMA"
Link: </articles/45>; rel="item"; class="articles"; id="45"; title="Architecture REST"
Link: </articles?page=2>; rel="next"; class="articles"; title="Page suivante"
```

### Article individuel

```http
GET /articles/42
Accept: application/json
```

```json
{
  "id": "42",
  "uri": "/articles/42",
  "titre": "Introduction à HONMA",
  "date_publication": "2024-02-01",
  "contenu": "HONMA est un protocole applicatif...",
  "statut": "publie",
  "tags": ["web", "api"],
  "auteur_id": "15"
}
```

```http
Link: </articles/42>; rel="canonical"; class="articles"; id="42"; title="Introduction à HONMA"
Link: </auteurs/15>; rel="author"; class="auteurs"; id="15"; title="Marie Martin"
Link: </articles/42/commentaires>; rel="collection"; class="commentaires"; title="Commentaires"
Link: </articles>; rel="up"; class="articles"; title="Articles"
```

### Collection hétérogène avec normalisation des espaces

```http
GET /feed
Accept: application/json
```

```json
[
  {
    "id": "42",
    "class": "article",
    "titre": "Introduction à HONMA"
  },
  {
    "id": "17",
    "class": "commentaire",
    "contenu": "Excellent article !"
  }
]
```

```http
# Ces trois en-têtes sont équivalents après normalisation des espaces
Link: </feed>; rel="self"; class="article   commentaire"; title="Fil d'actualité"
Link: </feed>; rel="self"; class=" article commentaire "; title="Fil d'actualité"
Link: </feed>; rel="self"; class="article commentaire"; title="Fil d'actualité"

# Le reste des liens de la réponse
Link: </articles/42>; rel="item"; class="article"; id="42"; title="Introduction à HONMA"
Link: </commentaires/17>; rel="item"; class="commentaire"; id="17"; title="Excellent article !"
Link: </feed?page=2>; rel="next"; class="article commentaire"; title="Page suivante"
```

## Métadonnées et structure

```http
OPTIONS /articles
Accept: application/json
```

```json
{
  "id": {
    "title": "Identifiant",
    "description": "Identifiant unique de l'article",
    "type": "string",
    "nullifiable": false,
    "example": "42"
  },
  "uri": {
    "title": "URI",
    "description": "URI canonique de l'article",
    "type": "string",
    "nullifiable": false,
    "example": "/articles/42"
  },
  "titre": {
    "title": "Titre",
    "description": "Titre principal de l'article",
    "type": "string",
    "nullifiable": false,
    "example": "Mon premier article"
  }
}
```

## Opérations de modification

### Création d'un article

```http
POST /articles
Content-Type: application/json

{
  "titre": "Nouvel article",
  "contenu": "Mon contenu...",
  "statut": "brouillon"
}
```

```http
Status: 201 Created
Location: /articles/47
Link: </articles/47>; rel="canonical"; class="articles"; id="47"; title="Nouvel article"
```

### Mise à jour d'un article

```http
PATCH /articles/42
Content-Type: application/json

{
  "statut": "publie",
  "date_publication": "2024-02-03"
}
```

```http
Status: 200 OK
Link: </articles/42>; rel="canonical"; class="articles"; id="42"; title="Introduction à HONMA"
```

## Gestion des erreurs

### Erreur de validation

```http
POST /articles
Content-Type: application/json

{
  "titre": "",
  "statut": "inconnu"
}
```

```http
Status: 422 Unprocessable Entity
Content-Type: application/json

{
  "titre": "Le titre ne peut pas être vide",
  "statut": "Valeurs autorisées : brouillon, publie"
}
```

## Requêtes conditionnelles

### Mise en cache

```http
GET /articles/42
If-None-Match: "a1b2c3"
```

```http
Status: 304 Not Modified
ETag: "a1b2c3"
```

### Modification concurrente

```http
PATCH /articles/42
If-Match: "a1b2c3"
Content-Type: application/json

{
  "titre": "Nouveau titre"
}
```

```http
Status: 412 Precondition Failed
ETag: "d4e5f6"
```
