# Spécification EWLP (Extended Web Linking Protocol)

## 1. Introduction

Dans une architecture REST, une même ressource peut être identifiée par différentes URIs, chacune représentant une perspective particulière de cette ressource. Par exemple, un article peut être identifié directement via `/articles/42` ou dans le contexte d'un commentaire via `/comments/123/article`. Cette multiplicité des identifiants peut créer une ambiguïté pour des clients qui doivent déterminer si différentes URIs identifient la même ressource.

EWLP étend le protocole Web Linking (RFC 8288) en ajoutant les attributs optionnels `class` et `id` aux liens. Ces attributs permettent respectivement de catégoriser les ressources et de les identifier de manière stricte.

## 2. Définitions

### Ressource

Une entité accessible via une ou plusieurs URIs. Une ressource peut être soit une collection, soit un membre.

### Collection

Un regroupement logique de zéro, un ou plusieurs membres, accessible via une URI. Une collection est toujours homogène : tous les membres sont de la même classe.

### Membre

Une ressource individuelle appartenant à une classe spécifique. Un membre est identifié de manière unique par la combinaison de ses attributs `class` et `id` lorsqu'ils sont présents.

### Classe

Pour une collection : indique le type unique des membres qu'elle contient.
Pour un membre : définit sa catégorie unique.

## 3. Contraintes

### 3.1 Collections

1. L'attribut `class` est RECOMMANDÉ et, lorsque présent, indique le type des membres contenus
2. L'attribut `class`, lorsque présent, DOIT contenir exactement un token
3. L'attribut `id` est DÉCONSEILLÉ
4. La classe, lorsque présente, DOIT être un token valide
5. Les espaces en début et fin sont ignorés

### 3.2 Membres

1. L'attribut `class` est RECOMMANDÉ
2. L'attribut `class`, lorsque présent, DOIT contenir exactement un token
3. L'attribut `id` est RECOMMANDÉ
4. La classe, lorsque présente, DOIT être un token valide
5. L'attribut `id`, lorsque présent, DOIT être un token valide
6. Les espaces en début et fin sont ignorés
7. Deux liens vers des membres avec la même combinaison {`class`, `id`} DOIVENT représenter la même ressource

## 4. Exemples

### 4.1 Collection avec tous les attributs recommandés

```http
# La classe "article" indique que la collection contient des articles
Link: </articles>; rel="collection"; class="article"; title="Articles",
      </articles/42>; rel="item"; class="article"; id="42"; title="Premier Article",
      </articles/57>; rel="item"; class="article"; id="57"; title="Second Article"
```

### 4.2 Même membre via différentes URIs

```http
Link: </articles/42>; rel="item"; class="article"; id="42"; title="Mon Article",
      </comments/3/article>; rel="canonical"; class="article"; id="42"; title="Le même article"
```

### 4.3 Collection paginée sans attribut id

```http
# Les pages d'une collection partagent la même classe car elles contiennent le même type de membres
Link: </articles>; rel="collection"; class="article"; title="Articles",
      </articles?page=2>; rel="next"; class="article"; title="Page suivante"
```

### 4.4 Collection minimale sans attributs optionnels

```http
Link: </articles>; rel="collection"; title="Articles",
      </articles/42>; rel="item"; title="Un Article"
```
