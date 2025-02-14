# Spécification SCION (Structured Class Identity Object Notation)

## 1. Définition

SCION est une extension de JSON permettant d'identifier et de catégoriser des ressources via les attributs `class` et `id`. Ce protocole se concentre uniquement sur l'identification des ressources, laissant la navigation et les liens à d'autres protocoles comme EWLP.

## 2. Types de Ressources

### 2.1 Collection

Un tableau JSON contenant zéro, un ou plusieurs membres. Une collection est toujours homogène : tous ses membres sont de la même classe.

### 2.2 Membre

Un objet JSON pouvant appartenir à une classe spécifique. Un membre est identifié de manière unique par son attribut `id` au sein d'une même classe.

## 3. Structure

### 3.1 Collections

Une collection :
- DOIT être un tableau JSON (`array`)
- NE PEUT PAS avoir d'attributs propres

### 3.2 Membres

Un membre :
- DOIT être un objet JSON (`object`)
- DOIT avoir un attribut `id`
- L'attribut `class` est RECOMMANDÉ

## 4. Exemples

### 4.1 Collection d'articles

```json
[
    {
        "class": "article",
        "id": "42",
        "title": "Premier Article",
        "summary": "Un article intéressant..."
    },
    {
        "class": "article",
        "id": "57",
        "title": "Second Article",
        "summary": "Un autre article fascinant..."
    }
]
```

### 4.2 Collection minimale

```json
[
    {
        "id": "42",
        "title": "Un Article",
        "summary": "Contenu de l'article..."
    }
]
```

### 4.3 Membre avec références

```json
{
    "class": "article",
    "id": "42",
    "title": "Mon Article",
    "content": "Le contenu de l'article...",
    "author": {
        "class": "user",
        "id": "15"
    }
}
```
