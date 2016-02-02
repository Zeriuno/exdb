SELECT    - Fonctions de calcul possibles
FROM      - Pas de fonctions de calcul
WHERE     - Pas de fonctions de calcul
GROUP BY  - Fonctions de calcul possibles
HAVING    - Fonctions de calcul possibles
ORDER BY (dont le critère doit être dans le select)  - Fonctions de calcul possibles

Client(*nom*, age, ville)
Produit(*code*, libelle, prix)
Commande (*nomClient-, codeProduit-*, quantite)


Pour client, conter les commandes et la quantité maximale commandée

SELECT nomClient, count(*), max(quantité)
FROM Commande
GROUP BY nomClient

Dans un GROUP BY on n'affiche que des attributs du groupe et des calculs.

Dqns un SELECT d'un groupe complet, pas d'attributs (c'est un GROUP BY implicite, sans aucun critère)
Pour client, conter les commandes et la quantité maximale commandée

Jamais de fonctions d'aggrégation dans un WHERE.

SELECT nomClient, count(*), max(quantité)
FROM Commande
GROUP BY nomClient

SELECT nomClient, count(*), max(quantite)
FROM Commande
WHERE quantite = ( SELECT max (quantite)
                   FROM Commande
);

Client ayant commandé tous les produits

SELECT nomClient
FROM Commande
GROUP BY nomClient
HAVING COUNT(DISTINCT codeProduit) = (SELECT COUNT(*)
                                      FROM Produit);
Personne, aucun, jamais <--- Différence


Client(*nom*, age, ville)
Produit(*code*, libelle, prix, villeFabrication)
Commande (*nomClient-, codeProduit-*, quantite)

Les villes où est un client (1) ou où sont fabriqués des produits (2).

(1)
SELECT ville
FROM Client

(2)
SELECT villeFabrication
FROM Produit

SELECT ville
FROM Client
UNION
SELECT villeFabrication
FROM Produit
