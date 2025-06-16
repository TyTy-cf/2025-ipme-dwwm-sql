
Explication de la base :
- address : les adresses des utilisateurs
- category : les categories des produits vendus
- characteristic : les caractéristiques possible des produits, comme la "Matière", "La taille", "Le poids", etc
- comment : les commentaires des produits
- order : les commandes des utilisateurs
- product : les produits en ventes 
- product_categories : les catégories liés aux produits
- product_characteristic : les caractéristiques des produits
- product_order : les produits contenu dans une commande
- user : les utilisateurs
  
(PS : le price est en centimes, là encore il faut le diviser par 100 pour l'avoir en €, il faudra le faire à chaque fois que le prix sera demandé)

Récapitulatif des "status" de commande :
- 100 : payée
- 110 : en cours de préparation
- 120 : livraison lancée
- 130 : en cours de livraison
- 140 : livrée

### 1/ Affichez les 9 produits les plus vendus, avec seulement les colonnes "product.id", "product.label", "product.price", "product.reference"

```sql
SELECT 	product.id,
        product.label,
        product.price,
        product.reference,
        SUM(product_order.quantity) as qty

FROM product
JOIN product_order ON product_order.product_id = product.id
 
GROUP BY product.label

ORDER BY qty DESC
LIMIT 9;
```

### 2/ Affichez les 4 catégories les plus vendus, avec seulement les colonnes "category.label", "category.id"

```sql
SELECT 	category.id,
        category.label,
        SUM(product_order.quantity) as qty

FROM category

JOIN product_categories ON product_categories.categories_id = category.id
JOIN product ON product.id = product_categories.products_id
JOIN product_order ON product_order.product_id = product.id
 
GROUP BY category.label

ORDER BY qty DESC
LIMIT 4;
```

### 3/ Affichez les 9 produits vendus dernièrement, avec seulement les colonnes "product.id", "product.label", "product.price", "product.reference"

### 4/ Affichez les 4 derniers commentaires postés, avec seulement les colonnes : "product.id", "user.id", "user.username", "product.label", "review.rating", "review.created_at"

### 5/ Affichez les informations de l'utilisateur avec ses adresses, avec seulement les colonnes : "user.id", "user.username", "user.first_name", "user.last_name", "address.*"

### 6/ Affichez le ou les utilisateurs ayant renseigné le plus d'adresses

### 7/ Afficher les produits de la catégorie "Books" et de ses sous-catégories

### 8/ Affichez les produits de la catégorie "Books" et de ses sous-catégories, mais cette fois, ceux de la page 4, on affichera 12 produits par page. De plus, afficher le total de produits à afficher, par exemple pour faire un affichage comme celui-là : "48 sur 168"

```sql
SELECT  product.id,
        product.label,
        c.label,
        (
           SELECT COUNT(DISTINCT product_categories.products_id)
           FROM product_categories, category parent
           LEFT JOIN category child ON child.parent_id = parent.id
           WHERE parent.label = 'Books'
           AND (
               product_categories.categories_id = parent.id
               OR
               product_categories.categories_id = child.id
           ) 
        ) AS "nbElements"

FROM product_categories

JOIN category c ON c.id = categories_id
JOIN product ON product_categories.products_id = product.id

WHERE c.parent_id = (
    SELECT id
    FROM category c2
    WHERE c2.label = 'Books'
)
OR c.id = (
    SELECT id
    FROM category c2
    WHERE c2.label = 'Books'
)

LIMIT 12  OFFSET 48;
```

### 9/ Affichez un récapitulatif des commandes de l'utilisateur, par exemple pour celui d'id "5". On affichera les colonnes suivantes : "order.address" (toutes les infos de celle-ci), "order.created_at", "order.status", "prix total de la commande" (à vous de trouver comment l'avoir... sachant que la colonne promotion est une réduction du prix de la commande en %)

### 10/ Affichez les produits relatifs à un autre, on considère que des produits sont relatifs l'un à l'autre lorsqu'ils ont la même catégorie. Prenez l'exemple avec l'id produit : 52

### 11/ Affichez toutes les catégories principales de l'application, par ordre alphabétique

### 12/ Affichez le nombre d'utilisateurs n'ayant pas validé leur inscription (colonne "activation_code" n'est pas null) depuis 1 ans

### 13/ Affichez les 3 mois les plus propices aux ventes, depuis le début du lancement de l'application (petit piège ici !)

### 14/ Affichez les 5 villes où le site livre le plus

### 15/ Affichez les 5 marques les plus vendus

### 16/ Affichez notre meilleur vendeur (un seul suffit ici !)

### 17/ Affichez 4 produits en adéquation avec la géolocation de l'utilisateur. L'utilisateur a une adresse à "Clermont-Ferrand", recommandez lui les 4 produits les plus achetés par des utilisateurs ayant une adresse à Clermont-Ferrand 
