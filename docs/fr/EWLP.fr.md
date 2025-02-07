# Spécification EWLP (Extended Web Linking Protocol)

## 1. Définition

EWLP est une extension du protocole Web Linking (RFC 8288) permettant d'augmenter les liens ressources avec les attributs `class` et `id`.

## 2. Structure

Un lien EWLP peut inclure deux attributs additionnels :
- `class` de type `string`
- `id` de type `string`

## 3. Contraintes

1. Un lien ressource doit inclure l'attribut `class`
2. L'attribut `id` est optionnel
3. La combinaison {`id`, `class`} identifie une ressource unique
4. Plusieurs liens peuvent partager la même combinaison {`id`, `class`} s'ils pointent vers la même ressource
5. Les valeurs de `class` et `id` doivent suivre le format défini dans RFC 8288 pour les paramètres de liens

## 4. Exemples

### 4.1 Lien ressource

```http
Link: </articles>; rel="collection"; class="articles"
```

### 4.2 Lien non-ressource

```http
Link: </css/main.css>; rel="stylesheet"
```

### 4.3 Liens multiples vers la même ressource

```http
Link: </articles/42>; rel="canonical"; class="articles"; id="42",
      </comments/123/article>; rel="item"; class="articles"; id="42"
```
