# Spécification ClassID-JSON

## 1. Définition

ClassID-JSON est une extension de JSON permettant d'augmenter un objet avec les attributs `class` et `id`.

## 2. Structure

Un objet JSON augmenté contient :

- Un attribut `class` (obligatoire)
- Un attribut `id` (optionnel)

## 3. Contraintes

1. `class` est de type `string`
2. `id` est de type `string`
3. La combinaison {`id`, `class`} doit être unique globalement

## 4. Exemples

### 4.1 Objet simple

```json
{
    "class": "article",
    "id": "42",
    "title": "Mon Article"
}
```

### 4.2 Document avec objets augmentés et non augmentés

```json
{
    "class": "article",
    "articles": [
        {
            "class": "article",
            "id": "1",
            "title": "Premier article"
        }
    ],
    "metadata": {
        "page": 1
    }
}
```
