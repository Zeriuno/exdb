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
where VilleB = 'PARIS' ;
```
4. Liste des buveurs (numéros et noms) qui habitent Paris et des buveurs qui habitent Macon.<br />
```
select NumBuveur, NomB
where VilleB = 'PARIS' OR VilleB = 'MACON' ;
```
5. Affichez les crus des vins de la région 'LOIRE' (sans la clause distinct et avec la clause distinct).<br />
```
show tables from tpvins ;
describe Vin ;
select Cru from Vin
where Region = 'LOIRE' ;
select distinct Cru from Vin
where Region = 'LOIRE' ;
```
6. Liste des différentes villes où habitent les buveurs.<br />
`select distinct VilleB from Buveur ;`

7. Liste des numéros de commande où la quantité commandée est comprise entre 10 et 50 (avec la clause between et sans la clause between).<br />
```
select NumCommande from Commande
where QtteCommandee > 10 and QtteCommandee < 50 ;
```
```
select NumCommande from Commande
where QtteCommandee between 10 and 50 ;
```
8. Liste des numéros de commandes livrées après le 1er décembre 1987.
```
show tables from tpvins ;
describe Livraison ;
select NumCommande from Livraison
where DateLiv > '1987-12-1' ;
```
9. Liste des vins (n° et cru) dont le cru commence par la lettre B<br />
```
describe Vin ;
select NumVin, Cru from Vin
where Cru like 'A%' ;
```
10.Liste des viticulteurs (n° et nom) dont le nom contient 'LIN'<br />
```
show tables from tpvins ;
describe Viticulteur ;
select NumVitic, NomV from Vin
where NomV like '%LIN%' ;
```
