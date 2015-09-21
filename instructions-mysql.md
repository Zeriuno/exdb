#Instructions MySQL

##Démarrrer

1. Montrer toutes les BDD à l'intérieur du SGBD<br />
`show databases ;`

2. Utiliser une spécifique BDD<br />
`use $database_name ;`

3. Montrer les tables à l'intérieur d'une BDD<br />
`show tables [from $database_name];`

4. Décrire une table<br />
`describe $table_name ;`


##Manipulation du contenu

##Requêtes

Ordre des éléments

```
SELECT
FROM
WHERE (ici s'embriquent les requêtes)
GROUP BY
HAVING
ORDER BY
```

###Opérateurs

####Opérateurs rélationnels

```
=
<=
>=
<>
<
>
```
####Opérateurs arithmétiques

```
+
-
*
/
```
#####Division

Trouver les buveurs qui ont commandé tous les vins -> division (tous est indice).

Requête + deux sous-requêtes NOT EXIST (trois blocs SELECT).

-> SELECT les noms des buveurs pour lesquels il n'existe pas de vin pour lequel il n'existe pas de commande pour ce buveur (lien Commande-Buveur)  et pour ce vin (lien Commande-Vin).
```
SELECT b.*
FROM Buveur b
WHERE NOT EXISTS (
  SELEC v.*
  FROM Vin n
  WHERE NOT EXISTS (
    SELECT c.*
    FROM Commande c
    WHERE c.NumBuveur = b.NumBuveur
    AND c.NumVin = n.NumVin
    )
  );
```
####Opérateurs logiques

```
AND
OR
NOT
```
Différence: NOT IN ou NOT EXISTS
Intersection: IN ou EXISTS ou jointure.
Division: 2 NOT EXISTS

####Opérateurs de calcul

```
AVG
MAX()
```

####Autres

```
LIKE
BETWEEN
IS NULL
IN
EXISTS
```
###Selecteurs

```
*
%
```

###SELECT

```
SELECT
SELECT DISTINCT
SELECT AS
```
###WHERE

```
WHERE
WHERE x AND y
WHERE x OR Y
WHERE x LIKE %y%
WHERE x BETWEEN y AND z
```

###Jointure

```
SELECT * FROM table t, autre a
WHERE t.numéro = a.numéro ;
```

###Traitement résultats

```
GROUP BY
HAVING
ORDER BY
```


###Requêtes imbriquées

####IN

```
SELECT b1.*
FROM Buveur b1
WERE b1.NumBuveur <> 1400
AND b1.VilleB IN
(SELECT b2.VilleB FROM Buveur b2
  WHERE b2.umBuveur = 1400) ;
```

####EXISTS

```
SELECT b1.*
FROM Buveur b1
WERE b1.NumBuveur <> 1400
AND EXISTS
 (SELECT b2.*
   FROM Buveur b2
  WHERE b2.NumBuveur = 1400
  AND b2.VilleB = b1.VilleB) ;
```

###Exemples

```
select Nom as 'Allons voir' from Buveur order by Nom asc ;
plusieurs attributs: X, Y ;
chaîne de caractères: 'Chaîne'
contient X: like %X%
select X from Y where attribute between 'A%' and 'G%'
différent: <>
not in (x,y)
```
