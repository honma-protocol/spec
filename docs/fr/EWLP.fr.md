# Spécification EWLP (Extended Web Linking Protocol)

## 1. Introduction

Dans une architecture REST, une même ressource peut être identifiée par différentes URIs, chacune représentant une perspective particulière de cette ressource. Par exemple, un article peut être identifié directement via `/articles/42` ou dans le contexte d'un commentaire via `/comments/123/article`. Cette multiplicité des identifiants peut créer une ambiguïté pour des clients qui doivent déterminer si différentes URIs identifient la même ressource.

EWLP étend le protocole Web Linking (RFC 8288) en ajoutant les attributs optionnels `class` et `id` aux liens. Ces attributs permettent respectivement de catégoriser les ressources et de les identifier de manière strict.

## 2. Définitions

### Ressource

Une entité accessible via une ou plusieurs URIs. Une ressource peut être soit une collection, soit un membre.

### Collection

Une ressource identifiée par son URI et regroupant zéro, un ou plusieurs membres. Une collection peut être homogène (tous les membres sont de même classe) ou hétérogène (les membres peuvent être de classes différentes).

### Membre

Une ressource individuelle appartenant à une classe spécifique.

### Classe

Pour une collection : indique le ou les types des membres qu'elle contient.
Pour un membre : définit sa catégorie unique.

### Identité

L'identité d'une ressource est définie selon son type :
1. Pour un membre : la combinaison stricte {`class`, `id`}
2. Pour une collection : son URI uniquement

## 3. Contraintes

### 3.1 Collections

1. L'attribut `class` est REQUIS et indique uniquement les types des membres contenus
2. L'attribut `class` PEUT contenir plusieurs classes séparées par des espaces
3. L'attribut `id` est INTERDIT
4. Chaque classe DOIT être un token valide
5. Les espaces en début et fin sont ignorés
6. Les espaces multiples sont normalisés en un seul espace

### 3.2 Membres

1. L'attribut `class` est REQUIS
2. L'attribut `class` DOIT contenir exactement une classe
3. L'attribut `id` est REQUIS
4. La classe DOIT être un token valide
5. L'attribut `id` DOIT être un token valide
6. Les espaces en début et fin sont ignorés

### 3.3 Identité

Deux liens vers des membres avec la même combinaison {`class`, `id`} DOIVENT représenter la même ressource

## 4. Exemples

### 4.1 Collection homogène

```http
# La classe "article" indique que la collection contient des articles
Link: </articles>; rel="collection"; class="article"; title="Articles",
      </articles/42>; rel="item"; class="article"; id="42"; title="Premier Article",
      </articles/57>; rel="item"; class="article"; id="57"; title="Second Article"
```

### 4.2 Collection hétérogène

```http
# Les classes "article comment" indiquent que la collection contient des articles et des commentaires
Link: </feed>; rel="collection"; class="article comment"; title="Fil d'actualité",
      </articles/42>; rel="item"; class="article"; id="42"; title="Un Article",
      </comments/17>; rel="item"; class="comment"; id="17"; title="Un Commentaire"
```

### 4.3 Même membre via différentes URIs

```http
Link: </articles/42>; rel="item"; class="article"; id="42"; title="Mon Article",
      </comments/3/article>; rel="canonical"; class="article"; id="42"; title="Le même article"
```

### 4.4 Collection paginée

```http
# Les pages d'une collection partagent la même classe car elles contiennent le même type de membres
Link: </articles>; rel="collection"; class="article"; title="Articles",
      </articles?page=2>; rel="next"; class="article"; title="Page suivante"
```
