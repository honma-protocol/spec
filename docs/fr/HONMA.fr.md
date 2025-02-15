# HONMA (HTTP Oriented Native Media Architecture)

## 1. Introduction

HONMA est un protocole applicatif pour la conception d'APIs Web reposant sur cinq protocoles :

1. HTTP et l'architecture REST
2. RRP (Resource Representation Protocol) pour la structuration des ressources
3. EWLP (Extended Web Linking Protocol) pour la navigation et l'identification des ressources
4. SCION (Structured Class Identity Object Notation) pour la représentation des données
5. RAPID (Resource API Protocol for Introspection and Discovery) pour l'introspection

## 2. Représentation des Ressources

### 2.1 Format et Types MIME

- Le seul format supporté est JSON
- Les types MIME acceptés sont :
  - `application/json`
  - `application/vnd.honma+json`

### 2.2 Implémentation RRP

Les ressources suivent la spécification RRP avec les contraintes supplémentaires suivantes :
- Une collection DOIT être représentée comme un tableau JSON (array)
- Un membre DOIT être représenté comme un objet JSON (hash)

## 3. Gestion des Erreurs

La gestion des erreurs dans HONMA suit la spécification RFC 9457 (Problem Details for HTTP APIs).

### 3.1 Format des Erreurs

Les réponses d'erreur DOIVENT :
- Utiliser le type MIME `application/problem+json`
- Contenir un objet JSON respectant la structure Problem Details
- Inclure au minimum les champs obligatoires selon RFC 9457

### 3.2 Structure de l'Erreur

Chaque erreur DOIT inclure :
- Un attribut `type` : URI identifiant le type d'erreur
- Un attribut `title` : Description courte et lisible du problème
- Un attribut `status` : Code HTTP de l'erreur (doit correspondre au code de la réponse HTTP)

Les champs optionnels suivants PEUVENT être inclus :
- `detail` : Explication détaillée spécifique à cette occurrence
- `instance` : URI identifiant l'occurrence spécifique de l'erreur

### 3.3 Exemples d'Erreurs

```http
HTTP/1.1 403 Forbidden
Content-Type: application/problem+json
Content-Language: fr

{
  "type": "https://api.example.com/problems/insufficient-credit",
  "title": "Crédit insuffisant",
  "status": 403,
  "detail": "Votre solde actuel est de 30€, mais cette opération nécessite 50€",
  "instance": "/transactions/12345",
  "balance": 30
}
```

## 4. Interactions

### 4.1 Négociation de Contenu

Les clients DOIVENT spécifier leur préférence de format via l'en-tête `Accept` :

```http
Accept: application/json
```

ou

```http
Accept: application/vnd.honma+json
```

### 4.2 Envoi de Données

Les clients DOIVENT utiliser un des types MIME supportés dans l'en-tête `Content-Type` :

```http
Content-Type: application/json
```

ou

```http
Content-Type: application/vnd.honma+json
```

## 5. Intégration des Protocoles

### 5.1 Navigation (EWLP)

- La navigation s'effectue exclusivement via les liens HTTP
- Les liens respectent la spécification EWLP
- Les attributs `class` et `id` sont obligatoires selon les règles EWLP

### 5.2 Représentation (SCION)

- Les ressources respectent la structure SCION
- Les attributs `class` et `id` sont requis selon les règles SCION
- Les références entre ressources suivent le format SCION

### 5.3 Introspection (RAPID)

- L'introspection s'effectue via la méthode OPTIONS
- Les réponses suivent le format RAPID
- Les métadonnées respectent la structure RAPID
