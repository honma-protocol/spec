# Spécification SCION
(Structured Class Identity Object Notation)

## 1. Définition

SCION est une extension de JSON permettant d'identifier et de catégoriser des ressources via les attributs `class` et `id`. Ce protocole se concentre uniquement sur l'identification des ressources, laissant la navigation et les liens à d'autres protocoles comme EWLP.

## 2. Types de Ressources

### 2.1 Collection

Une ressource regroupant zéro, un ou plusieurs membres. Une collection peut contenir :
- Des membres d'une même classe (collection homogène)
- Des membres de classes différentes (collection hétérogène)

### 2.2 Membre

Une ressource individuelle appartenant à une classe spécifique.

## 3. Structure

### 3.1 Collections

Une collection DOIT contenir :
- Un attribut `class` : chaîne de caractères listant la ou les classes des membres contenus

Une collection NE DOIT PAS contenir :
- Un attribut `id`

### 3.2 Membres

Un membre DOIT contenir :
- Un attribut `class` : chaîne de caractères définissant sa classe
- Un attribut `id` : chaîne de caractères définissant son identifiant unique dans sa classe

## 4. Contraintes

1. Une collection peut déclarer plusieurs classes dans son attribut `class`
2. Un membre ne peut déclarer qu'une seule classe dans son attribut `class`
3. Deux membres ayant la même combinaison {`class`, `id`} représentent la même ressource

## 5. Exemples

### 5.1 Collection homogène

```json
{
    "class": "article",
    "items": [
        {
            "class": "article",
            "id": "42",
            "title": "First Article",
            "summary": "An interesting article..."
        },
        {
            "class": "article",
            "id": "57",
            "title": "Second Article",
            "summary": "Another fascinating article..."
        }
    ]
}
```

### 5.2 Collection hétérogène

```json
{
    "class": "article comment",
    "items": [
        {
            "class": "article",
            "id": "42",
            "title": "An Article",
            "summary": "Article content..."
        },
        {
            "class": "comment",
            "id": "17",
            "content": "A Comment",
            "date": "2024-02-13"
        }
    ]
}
```

### 5.3 Membre avec références

```json
{
    "class": "article",
    "id": "42",
    "title": "My Article",
    "content": "The article content...",
    "author": {
        "class": "user",
        "id": "15"
    },
    "comments": {
        "class": "comment",
        "items": [
            {
                "class": "comment",
                "id": "17",
                "content": "First comment",
                "date": "2024-02-13"
            }
        ]
    }
}
```
