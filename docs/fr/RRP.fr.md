# Resource Representation Protocol (RRP)

1. Types de Ressources
   - Une ressource HTTP est soit un membre, soit une collection
   - Un membre est une ressource individuelle
   - Une collection est un regroupement logique de membres

2. Identification
   - Chaque ressource est identifiée par son URI
   - Convention optionnelle : les URIs de collection se terminent par "/"

3. Représentation
   - Un membre doit être représenté comme une structure de données plate contenant des paires clé/valeur
   - Une collection doit être représentée comme une séquence ordonnée de membres
