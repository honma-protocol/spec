# Spécification EWLP (Extended Web Linking Protocol)

## 1. Introduction

Dans une architecture REST, une même ressource peut être identifiée par différentes URIs, chacune représentant une perspective particulière de cette ressource. Par exemple, un article peut être identifié directement via `/articles/42` ou dans le contexte d'un commentaire via `/comments/123/article`. Cette multiplicité des identifiants, bien que valide, crée une ambiguïté pour les clients qui doivent déterminer si différentes URIs identifient la même ressource.

EWLP résout cette ambiguïté en étendant le protocole Web Linking (RFC 8288) avec les attributs `class` et `id`. La combinaison de ces attributs fournit un identifiant stable de la ressource, indépendant des URIs qui deviennent alors des identifiants contextuels. Cette approche permet aux clients de reconnaître une même ressource à travers différents contextes d'identification, tout en maintenant le principe REST d'opacité des URIs.

## 2. Structure

Un lien EWLP peut inclure deux attributs additionnels :

- `class` de type `string` : plusieurs valeurs possibles
- `id` de type `string` : une seule valeur possible

## 3. Contraintes

1. Les attributs `class` et `id` sont optionnels
2. L'attribut `class` est requis uniquement en cas d'ambiguïté (de collision) sur l'`id`
3. Deux liens partageant le même `id` et la même `class` identifient la même ressource

## 4. Exemples

### 4.1 Lien ressource simple

```http
Link: </articles>; rel="collection"; class="articles"
```

### 4.2 Lien non-ressource

```http
Link: </css/main.css>; rel="stylesheet"
```

### 4.3 Liens multiples vers la même ressource

```http
Link: </articles/42>; rel="canonical"; class="article"; id="42";  title="Article #42",
      </comments/123/article>; rel="item"; class="article"; id="42"; title="Article commenté"
```

Dans cet exemple, les deux liens identifient la même ressource (l'article numéro 42). Chaque URI offre une perspective différente sur cette ressource :
- Via la collection des articles
- Dans le contexte d'un commentaire

### 4.4 Collection avec classes multiples

```http
Link: </blog>; rel="collection"; class="articles posts"
```

## 5. Note additionnelle

- Une collection peut avoir une `id`.
