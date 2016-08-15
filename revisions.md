SELECT    - Fonctions de calcul possibles
FROM      - Pas de fonctions de calcul
WHERE     - Pas de fonctions de calcul
GROUP BY  - Fonctions de calcul possibles
HAVING  (seulement associé à GROUP BY)  - Fonctions de calcul possibles
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



Client(*nom*, age, ville)
Produit(*code*, libelle, prix, villeFabrication)
Commande (*nomClient-, codeProduit-*, quantite, date)

1 - Quel client n'a pas commandé de produits aujourd'hui?
2 - Quel client est le plus âgé?
3 - Dans quelles villes n'est fabriqué aucun produit de plus de 10€?


1.

/*NOPE!
SELECT nomClient
FROM Commande
WHERE date <> 2016-02-02 ;*/ <-- Celui ayant commandé hier et aujourd'hui y sera présent.

SELECT nom
FROM Client
WHERE nom NOT IN (SELECT nomClient
                  FROM Commande
                  WHERE date = 2016-02-02);

SELECT nom
FROM Client l
WHERE NOT EXISTS (SELECT *
                  FROM Commande o
                  WHERE date = 2016-02-02
                  AND o.nomClient = l.nom);

SELECT nom
FROM Client
EXCEPT
SELECT nomClient
FROM Commande
WHERE date = 2016-02-02


2.

SELECT nomClient
FROM Client
WHERE age = ( SELECT max(age)
               FROM Client);

Ou bien

SELECT nomClient
FROM Client
WHERE NOT EXISTS (SELECT *
                  FROM Client)

3.

SELECT villeFabrication
FROM Produit p
WHERE NOT EXISTS (SELECT *
                  FROM Produit r
                  WHERE prix > 10
                  AND r.villeFabrication = p.villeFabrication);

SELECT villeFabrication
FROM Produit
WHERE villeFabrication NOT IN(SELECT villeFabrication
                              FROM Produit
                              WHERE prix > 10)


*****
PLSQL

Client(*nom*, age, ville)
Produit(*code*, libelle, prix, stock)
Commande (*nomClient-, codeProduit-*, quantite)


1 Quantité inférieure à 1000
2 Un produit ne peut pas être commandé deux fois le même jour
3 Le prix d'un attribut doit toujours être renseigné
4 Un client ne peut habiter qu'à Paris, Lyon ou Nantes
5 Une commande ne peut excéder le stock restant du produit
6 La quantité totale commandée pour un produit doit être inférieure à 10.000


1 - Contrainte d'integrité: CHECK(quantite < 1000)
2 - Contrainte d'unicité: UNIQUE(codeProduit, date)
3 - Contrainte d'integrité: NOT NULL
4 - Contrainte d'integrité: CHECK(ville in (Paris, Lyon, Nantes))
5 - Trigger.
6 - Trigger.

5

CREATE OR REPLACE Trigger T5
BEFORE INSERT ON Commande
FOR EACH ROW /* pour ne pas contrôler tout un bloc de mises à jour d'un coup, mais ligne par ligne */

DECLARE
  v_stock Produit.stock%TYPE;
BEGIN
SELECT stock INTO v_stock
FROM Commande c, Produit p
WHERE p.code = :new.codeProduit
IF(v_stock < :new.quantite)
THEN raise_application_error (-20002, 'Commande excédant le stock du produit');
END IF;
END;
/


Ecrire une procédure affichant combien rapporte chaque produit.


SELECT codeProduit, sum(quantite*prix)
FROM Produit, Commande
WHERE codeproduit = code
GROUP BY codeproduit

CREATE OR REPLACE PROCEDURE toto /pas de paramètre/ IS
CURSOR liste_ventes IS
  SELECT codeProduit, sum(quantite*prix) as total
  FROM Produit, Commande
  WHERE codeproduit = code
  GROUP BY codeproduit
v_totalproduit liste_ventes%ROWTYPE    ;
BEGIN
  OPEN liste_ventes                    ;
  FETCH liste_ventes intov_gainProduit ;
  WHILE(liste_ventes%FOUND) LOOP
  DBMS_PUT_LINE('Le produit ' || v_gainProduit.code || ' a rappporté ' || v_gainProduit.total);
  FETCH liste_ventes INTO v_gainProduit ;
END LOOP;
CLOSE liste_ventes;
END;
