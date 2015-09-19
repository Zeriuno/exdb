#Instructions MySQL

Démarrrer

1. Montrer toutes les BDD à l'intérieur du SGBD<br />
`show databases ;`

2. Utiliser une spécifique BDD<br />
`use $database_name ;`

3. Montrer les tables à l'intérieur d'une BDD<br />
`show tables from $database_name ;`

4. Décrire une table<br />
`describe $table_name ;`


##SELECT

```
SELECT
SELECT DISTINCT
SELECT AS
```

```
select Nom as 'Allons voir' from Buveur order by Nom asc ;
plusieurs attributs: X, Y ;
chaîne de caractères: 'Chaîne'
contient X: like %X%
select X from Y where attribute between 'A%' and 'G%'
différent: <>
not in (x,y)
```
