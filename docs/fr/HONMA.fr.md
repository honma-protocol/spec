# HONMA (HTTP Oriented Native Media Architecture)

## 1. Introduction

HONMA est un protocole applicatif pour la conception d'APIs Web reposant sur quatre protocoles :

1. HTTP et l'architecture REST
2. EWLP (Extended Web Linking Protocol) pour la navigation et l'identification des ressources
3. SCION (Structured Class Identity Object Notation) pour la représentation des données
4. RAPID (Resource API Protocol for Introspection and Discovery) pour l'introspection

## 2. Représentation des Ressources

### 2.1 Format et Types MIME

- Le seul format supporté est JSON
- Les types MIME acceptés sont :
  - `application/json`
  - `application/vnd.honma+json`

### 2.2 Structure

1. Une représentation de ressource est soit une collection, soit un membre
2. Une collection DOIT être représentée comme un tableau JSON (array)
3. Un membre DOIT être représenté comme un objet JSON (hash)

## 3. URI et Convention de Nommage

### 3.1 Collections

- Les URI de collection DEVRAIENT se terminer par un slash
- Exemple : `/articles/`, `/users/`, `/comments/`

### 3.2 Membres

- Les URI de membre NE DEVRAIENT PAS se terminer par un slash
- Exemple : `/articles/42`, `/users/15`, `/comments/7`

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
