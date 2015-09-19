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

####Opérateurs logiques

```
AND
OR
NOT
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
