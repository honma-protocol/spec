# Spécification EWLP (Extended Web Linking Protocol)

## 1. Introduction

Dans une architecture REST, une même ressource peut être identifiée par différentes URIs, chacune représentant une perspective particulière de cette ressource. Par exemple, un article peut être identifié directement via `/articles/42` ou dans le contexte d'un commentaire via `/comments/123/article`. Cette multiplicité des identifiants, bien que valide, crée une ambiguïté pour les clients qui doivent déterminer si différentes URIs identifient la même ressource.

EWLP résout cette ambiguïté en étendant le protocole Web Linking (RFC 8288) avec les attributs `class` et `id`. La combinaison de ces attributs fournit un identifiant stable de la ressource, indépendant des URIs qui deviennent alors des identifiants contextuels. Cette approche permet aux clients de reconnaître une même ressource à travers différents contextes d'identification, tout en maintenant le principe REST d'opacité des URIs.

## 2. Structure

Un lien EWLP peut identifier aussi bien une ressource individuelle qu'une collection. Il peut inclure deux attributs additionnels :

- `class` de type `string` : identifie le type de la ressource. Plusieurs valeurs possibles.
- `id` de type `string` : identifie de manière unique une ressource (membre ou collection) au sein de sa classe. Une seule valeur possible.

Une ressource peut être :
- Un membre : ressource individuelle (ex: un article spécifique)
- Une collection : ensemble de ressources qui peuvent être de natures différentes (ex: un flux d'activités contenant des articles et des commentaires)

## 3. Contraintes

1. Les attributs `class` et `id` sont optionnels
2. L'attribut `class` est requis uniquement en cas d'ambiguïté (de collision) sur l'`id`
3. Deux liens partageant le même `id` et la même `class` identifient la même ressource
4. Une collection peut avoir un `id` tout comme un membre
5. Les membres d'une collection peuvent avoir des `class` différentes

## 4. Exemples

### 4.1 Lien vers une collection homogène

```http
Link: </articles>; rel="collection"; class="articles"; id="main"
```

### 4.2 Lien vers une collection hétérogène

```http
Link: </feed>; rel="collection"; class="feed"; id="user-activity",
      </feed/1>; rel="item"; class="article"; id="42"; title="Nouvel article",
      </feed/2>; rel="item"; class="comment"; id="17"; title="Commentaire récent"
```

### 4.3 Lien vers un membre

```http
Link: </articles/42>; rel="canonical"; class="article"; id="42"; title="Article #42"
```

### 4.4 Lien non-ressource

```http
Link: </css/main.css>; rel="stylesheet"
```

### 4.5 Liens multiples vers la même ressource

```http
Link: </articles/42>; rel="canonical"; class="article"; id="42"; title="Article #42",
      </comments/123/article>; rel="item"; class="article"; id="42"; title="Article commenté"
```

Dans cet exemple, les deux liens identifient la même ressource (l'article numéro 42). Chaque URI offre une perspective différente sur cette ressource :
- Via la collection des articles
- Dans le contexte d'un commentaire

### 4.6 Ressource avec classes multiples

```http
Link: </blog/42>; rel="item"; class="feed article"; id="42"
```
