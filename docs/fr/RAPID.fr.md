# RAPID (Resource API Protocol for Introspection and Discovery)

Protocole d'API pour l'Introspection et la Découverte des Ressources

## 1. Introduction

RAPID est un protocole d'introspection standardisé permettant aux APIs HTTP de décrire dynamiquement leurs ressources. En étendant la sémantique de la méthode OPTIONS, RAPID permet aux clients de découvrir automatiquement la structure, les contraintes et les capacités des ressources exposées.

## 2. Objectifs

- Permettre l'auto-description des APIs HTTP
- Standardiser la découverte des capacités des ressources
- Faciliter l'exploration dynamique des APIs
- Fournir une documentation intégrée et toujours à jour
- Améliorer l'interopérabilité entre clients et serveurs
- Réduire le couplage entre clients et serveurs

## 3. Concepts Fondamentaux

### 3.1 Introspection

L'introspection permet à une ressource de communiquer sa structure et ses contraintes via une interface standardisée. Cette capacité est essentielle pour :

- La découverte automatique des capacités
- La validation côté client
- L'adaptation dynamique des interfaces
- La génération de documentation

### 3.2 Découverte

La découverte permet aux clients de :

- Explorer l'API de manière progressive
- Adapter leur comportement aux capacités disponibles
- Évoluer sans rupture lors des changements de l'API
- Construire des interfaces dynamiques

## 4. Format du Document

### 4.1 Structure Générale

Une réponse RAPID comprend :

1. Un en-tête HTTP `Allow` listant les méthodes autorisées
2. Un corps JSON décrivant chaque méthode permise

Structure pour chaque méthode :
```json
{
  "METHOD": {
    "title": "Titre de l'opération",
    "description": "Description détaillée",
    "request": {
      "headers": {},
      "query_string": {},
      "body": {}
    },
    "response": {
      "headers": {},
      "body": {}
    }
  }
}
```

### 4.2 Description des Champs

Chaque champ est décrit par :

Attribut | Type | Obligatoire | Valeur par défaut | Description
---------|------|-------------|-------------------|-------------
`title` | chaîne | oui | "" | Titre court du champ
`description` | chaîne | oui | "" | Description détaillée
`type` | chaîne | oui | "string" | Type de données
`nullifiable` | booléen | non | true | Indique si la valeur null est acceptée
`restricted_values` | tableau | non | null | Liste des valeurs autorisées
`example` | * | non | null | Exemple de valeur valide

### 4.3 Types de Données

Type | Description | Contraintes Possibles
-----|-------------|---------------------
`string` | Chaîne de caractères | `minlen`, `maxlen`, `pattern`
`number` | Nombre | `min`, `max`
`boolean` | Booléen | -
`array` | Tableau | `minItems`, `maxItems`
`hash` | Objet | `required`, `properties`
`file` | Fichier | `maxSize`, `mimeTypes`

### 4.4 Contraintes Spécifiques

#### Chaînes (string)

```json
{
  "type": "string",
  "minlen": 5,
  "maxlen": 100,
  "pattern": "^[A-Za-z0-9]+$"
}
```

#### Nombres (number)

```json
{
  "type": "number",
  "min": 0,
  "max": 100
}
```

#### Valeurs Énumérées

```json
{
  "restricted_values": [
    {
      "value": "draft",
      "title": "Brouillon",
      "description": "Article en cours de rédaction"
    },
    {
      "value": "published",
      "title": "Publié",
      "description": "Article visible publiquement"
    }
  ]
}
```

## 5. Exemples d'Utilisation

### 5.1 Ressource Simple

```http
OPTIONS /articles/123
Accept: application/json
```

```http
HTTP/1.1 200 OK
Allow: OPTIONS, GET, PATCH, DELETE
Content-Type: application/json
```

```json
{
  "GET": {
    "title": "Consulter un article",
    "description": "Récupère les détails d'un article spécifique",
    "response": {
      "body": {
        "id": {
          "type": "string",
          "description": "Identifiant unique",
          "nullifiable": false
        },
        "title": {
          "type": "string",
          "description": "Titre de l'article",
          "maxlen": 200,
          "nullifiable": false
        },
        "content": {
          "type": "string",
          "description": "Contenu de l'article",
          "nullifiable": false
        },
        "status": {
          "type": "string",
          "description": "Statut de publication",
          "restricted_values": [
            {
              "value": "draft",
              "title": "Brouillon"
            },
            {
              "value": "published",
              "title": "Publié"
            }
          ],
          "nullifiable": false
        }
      }
    }
  }
}
```

### 5.2 Création de Ressource

```http
OPTIONS /articles
Accept: application/json
```

```json
{
  "POST": {
    "title": "Créer un article",
    "description": "Crée un nouvel article",
    "request": {
      "headers": {
        "Content-Type": {
          "type": "string",
          "description": "Type de contenu",
          "restricted_values": [
            {
              "value": "application/json",
              "title": "JSON"
            }
          ],
          "nullifiable": false
        }
      },
      "body": {
        "title": {
          "type": "string",
          "description": "Titre de l'article",
          "minlen": 1,
          "maxlen": 200,
          "nullifiable": false,
          "example": "Mon premier article"
        },
        "content": {
          "type": "string",
          "description": "Contenu de l'article",
          "nullifiable": false,
          "example": "Contenu de l'article..."
        }
      }
    }
  }
}
```

## 6. Types MIME

Les implémentations doivent utiliser un des types MIME suivants :

- `application/json`
- `application/vnd.rapid+json`

## 7. Considérations de Sécurité

- Les informations sensibles ne doivent pas être exposées dans les exemples
- Les contraintes de validation ne doivent pas révéler de détails d'implémentation
- L'accès aux métadonnées doit respecter les mêmes règles d'autorisation que les ressources

## 8. Références

- RFC 2119 - Mots-clés des RFC
- RFC 2616 - HTTP/1.1
- RFC 7159 - JSON
- RFC 8288 - Web Linking
