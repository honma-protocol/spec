# Spécification ClassID-JSON

## 1. Définition

ClassID-JSON est une extension de JSON permettant d'augmenter un objet avec les attributs `class` et `id`.

## 2. Structure

Un objet JSON augmenté contient :

- Un attribut `class` (obligatoire)
- Un attribut `id` (optionnel)

## 3. Contraintes

1. `class` est de type `string` et DOIT suivre les règles suivantes :
   - Une ou plusieurs classes séparées par des espaces
   - Chaque classe doit être un token valide selon les règles HTML
   - Les espaces en début et fin sont ignorés
   - Les espaces multiples sont normalisés en un seul espace

2. `id` est de type `string` et DOIT suivre les règles suivantes :
   - Un seul token valide selon les règles HTML
   - Les espaces en début et fin sont ignorés

3. La combinaison {`id`, `class`} identifie une ressource unique

4. Plusieurs objets peuvent partager la même combinaison {`id`, `class`} s'ils représentent la même ressource

## 4. Exemples

### 4.1 Objet simple

```json
{
    "class": "article",
    "id": "42",
    "title": "Mon Article"
}
```

### 4.2 Objets multiples pour la même ressource

```json
{
    "class": "article",
    "articles": [
        {
            "class": "article",
            "id": "42",
            "title": "Mon Article"
        }
    ],
    "latest": {
        "class": "article",
        "id": "42",
        "title": "Mon Article"
    }
}
```

### 4.3 Normalisation des espaces

```json
// Ces objets sont équivalents après normalisation
{
    "class": "article   comment",
    "id": "42"
}
{
    "class": " article comment ",
    "id": "42"
}
{
    "class": "article comment",
    "id": "42"
}
```
