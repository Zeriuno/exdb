#Fondamentaux

1. Montrer toutes les BDD à l'intérieur du SGBD<br />
`show databases ;`

2. Utiliser une spécifique BDD<br />
`use $database_name ;`

3. Montrer les tables à l'intérieur d'une BDD<br />
`show tables from $database_name ;`

4. Décrire une table<br />
`describe $table_name ;`

--
#tpvins

##Requêtes simples

1. Liste des buveurs.<br />
`select * from Buveur ;`

2. Liste des buveurs (n°, nom et ville)<br />
`select NumBuveur, NomB, VilleB from Buveur ;`

3. Liste des numéros et noms des buveurs habitant 'PARIS'<br />
```
select NumBuveur, NomB from Buveur
where VilleB = 'Paris' ;
```
4. 
