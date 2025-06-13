
Explication de la base :
- category : les categories de jeux, comme "FPS", "Action", etc
- country : les pays disponibles, les langues, drapeaux etc
- game : les jeux
- game_category : table relationnelle pour représenter les catégories dans lesquelles un jeu est disponible
- game_country : table relationnelle pour représenter les langues dans lesquelles un jeu est disponible
- publisher : les éditeurs de jeux
- review : les commentaire déposés par les utilisateurs sur un jeu
- user : les utilisateurs
- user_own_game : les jeux possédés par les utilisateurs

### 1/ Affichez les 9 derniers jeux sortis ("game.published_at")

### 2/ Affichez les 9 jeux les plus vendus (un jeu est considéré comme vendu, lorsque sa clé primaire est présente dans la table "user_own_game")

### 3/ Affichez les 9 jeux les plus joués (table "user_own_game", colonne "game_time")

### 4/ Affichez les 4 derniers commentaires postés, avec seulement ces colonnes : "user.nickname", "game.name", "review.rating", "review.up_vote", "review.down_vote"

### 5/ Affichez les 9 catégories les plus joués (va probablement falloir chercher dans "user_own_game"...)

### 6/ Affichez le temps de jeu total par nom de compte (colonne : "user.nickname" et "Temps de jeux total", le temps de jeu se trouve dans : "user_own_game.game_time")

### 7/ Afficher les jeux ayant la catégorie "FPS" et coûtant moins de 30€

### 8/ Afficher les jeux (table : "game") avec leurs différentes catégories respectives, cependant je veux une ligne par jeux et les différents genres en une seule colonne, que vous renommerez "Genre(s)". (PS : utiliser la fonction "GROUP_CONCAT" pour l'affichage en une seule colonne)

### 9/ Affichez le prix total des jeux achetés pour un utilisateur (colonne : "user.nickname" et "Coût total"

### 10/ Afficher par année, le nombre de jeu sortis pour chacune d'entre elles, par exemple : "2025 : 0", "2000 : 3", etc

### 11/ Afficher les jeux les plus anciens

### 12/ Afficher les jeux avec leur note respective (la note se trouve dans la table "review", colonne "rating"), on doit calculer la moyenne des "rating" par jeu

### 13/ Afficher le jeu le moins bien noté (la note la plus basse)

### 14/ Afficher les jeux dont leur note est supérieure à la moyenne globale

### 15/ Afficher les utilisateurs n'ayant jamais acheté de jeu

### 16/ Afficher les utilisateurs ayant acheté au moins un jeu qui n'est pas dans leur langue natale

### 17/ Afficher le ratio de présence des utilisateurs par pays

### 18/ Faire une requête qui simule une barre de recherche, elle doit récupérer les informations en provenance des tables "game", "category", "publisher". Par exemple si l'utilisateur tape "pro", on doit faire ressortir tous les jeux ayant spécifiquement le nom "pro" ou ceux qui contiennent la chaine de caractère "pro", pareil pour les gens et éditeurs de jeu. Vous devrez tester votre requête avec une valeur par défaut.

### 19/ Même question que la 18, mais cette fois en pondérant les résultats pour les trier par pertinence ?