# HONMA - Guide par l'exemple

Ce guide présente les différents aspects du protocole HONMA à travers des exemples concrets.

## Ressources et représentations

### Collection d'articles

```http
GET /articles
Accept: application/json
```

```json
[
  {
    "id": 42,
    "titre": "Introduction à HONMA",
    "date_publication": "2024-02-01",
    "statut": "publie"
  },
  {
    "id": 45,
    "titre": "Architecture REST",
    "date_publication": null,
    "statut": "brouillon"
  }
]
```

```http
Link: </articles/42>; rel="item"; title="Introduction à HONMA"; id="42"
Link: </articles/45>; rel="item"; title="Architecture REST"; id="45"
Link: </articles?page=2>; rel="next"; title="Page suivante"
```

La même collection peut être demandée dans un autre format :

```http
GET /articles
Accept: text/csv
```

```csv
id,titre,date_publication,statut
42,Introduction à HONMA,2024-02-01,publie
45,Architecture REST,,brouillon
```

### Article individuel

```http
GET /articles/42
Accept: application/json
```

```json
{
  "id": 42,
  "titre": "Introduction à HONMA",
  "date_publication": "2024-02-01",
  "contenu": "HONMA est un protocole applicatif...",
  "statut": "publie",
  "tags": ["web", "api"],
  "auteur_id": 15
}
```

```http
Link: </auteurs/15>; rel="author"; title="Marie Martin"; id="15"
Link: </articles/42/commentaires>; rel="collection"; title="Commentaires"
Link: </articles/42/versions>; rel="version-history"; title="Historique des versions"
```

## Navigation et relations

### Pagination

```http
GET /articles?page=2
Accept: application/json
```

```json
[
  {
    "id": 46,
    "titre": "Sécurité des API",
    "date_publication": "2024-01-15",
    "statut": "publie"
  }
]
```

```http
Link: </articles?page=1>; rel="prev"; title="Page précédente"
Link: </articles?page=3>; rel="next"; title="Page suivante"
Link: </articles?page=1>; rel="first"; title="Première page"
Link: </articles?page=5>; rel="last"; title="Dernière page"
Link: </articles/46>; rel="item"; title="Sécurité des API"; id="46"
```

### Filtrage et recherche

```http
GET /articles?statut=publie&tag=web
Accept: application/json
```

```json
[
  {
    "id": 42,
    "titre": "Introduction à HONMA",
    "date_publication": "2024-02-01",
    "statut": "publie"
  }
]
```

```http
Link: </articles?statut=publie>; rel="filter"; title="Articles publiés"
Link: </articles?tag=web>; rel="filter"; title="Articles web"
Link: </articles/recherche>; rel="search"; title="Recherche avancée"
Link: </articles/42>; rel="item"; title="Introduction à HONMA"; id="42"
```

## Métadonnées et structure

### Description des attributs

```http
OPTIONS /articles
Accept: application/json
```

```json
{
  "id": {
    "title": "Identifiant",
    "description": "Identifiant unique de l'article",
    "type": "integer",
    "nullifiable": false,
    "example": 42
  },
  "titre": {
    "title": "Titre",
    "description": "Titre principal de l'article",
    "type": "string",
    "nullifiable": false,
    "example": "Mon premier article"
  },
  "date_publication": {
    "title": "Date de publication",
    "description": "Date de publication de l'article",
    "type": "string",
    "nullifiable": true,
    "example": "2024-02-01"
  },
  "statut": {
    "title": "Statut",
    "description": "État de publication de l'article",
    "type": "string",
    "nullifiable": false,
    "restricted_values": [
      {
        "title": "Brouillon",
        "description": "Article en cours de rédaction",
        "value": "brouillon"
      },
      {
        "title": "Publié",
        "description": "Article visible publiquement",
        "value": "publie"
      }
    ],
    "example": "brouillon"
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
Link: </articles/47>; rel="self"; title="Nouvel article"; id="47"
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
Link: </articles/42>; rel="self"; title="Introduction à HONMA"; id="42"
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

### Ressource non trouvée

```http
GET /articles/999
Accept: application/json
```

```http
Status: 404 Not Found
Content-Type: application/json

{
  "message": "Article non trouvé"
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
