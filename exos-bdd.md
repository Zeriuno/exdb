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
10. Liste des viticulteurs (n° et nom) dont le nom contient 'LIN'<br />
```
show tables from tpvins ;
describe Viticulteur ;
select NumVitic, NomV from Vin
where NomV like '%LIN%' ;
```
11. Liste des buveurs (n° et nom) qui n'habitent ni Paris ni Macon (2 solutions dont 1 avec not in)<br />
```
describe Buveur ;
select NumBuveur and NomB from Buveur
where VilleB <> 'PARIS' and VilleB <> 'MACON' ;
```
```
select NumBuveur and NomB from Buveur
where VileB not in('PARIS', 'MACON') ;
```
##Requêtes avec jointures
12. Liste des buveurs (n°, nom et ville) qui ont passé au moins une commande (avec distinct et sans).<br />
```
show tables from tpvins ;
describe Commande ;
select Commande.NumBuveur, NomB, VilleB from Buveur, Commande
where Commande.NumBuveur = Buveur.NumBuveur ;
select distinct Commande.NumBuveur, NomB, VilleB from Buveur, Commande
where Commande.NumBuveur = Buveur.NumBuveur ;
```
avec alias
```
select c.NumBuveur, NomB, VilleB from Commande c, Buveur b, where c.NumBuveur = b.NumBuveur ;
```
13. Liste des viticulteurs (n°, nom et ville) qui proposent du vin de Loire de millésime 1983.<br />
```
describe Vin ;
describe Viticulteur ;
select v.NumVitic, NomV, VilleV from Viticulteur v, Vin n
where v.NumVitic = n.NumVitic and Region = "LOIRE" and Millesime = "1983" ;
```
14. Liste des buveurs (n° et nom) qui ont commandé du vin de cru "Pommard".<br />
```
describe Commande ;
describe Vin ;
describe Buveur ;
select b.NumBuveur, NomB from Buveur b, Commande c, Vin n
where b.NumBuveur = c.NumBuveur
and c.NumVin = n.NumVin
and Cru = "POMMARD"
```
15. Noms des viticulteurs à qui le buveur 1600 a comandé du vin (2 solutions, avec ou sans sous-requêtes).
```
select v.NomV from Viticulteur v, Vin n, Commande c, Buveur b
where b.NumBuveur = c.NumBuveur
and c.NumVin = n.NumVin
and n.NumVitic = v.NumVitic
and c.NumBuveur = 1600 ;
```
16. Noms des viticulteurs à qui le buveur Dupond a commandé du vin.<br />
```
describe Viticulteur ;
select NomV from Viticulteur v, Buveur b, Commande c, Vin n
where n.NumVitic = v.NumVitic
and b.NumBuveur = c.NumBuveur
and n.NumVin = c.NumVin
and NomBuveur = "DUPOND" ;
```
17. Liste des viticulteurs (n°, nom et ville) qui habitent la même ville que l'un de leurs clients.<br />

Selection de viticulteurs habitant dans la même ville que les buveurs (pas leurs clients).
```
SELECT DISTINCT NumVitic, NomV, VilleV from Viticulteur, Buveur
WHERE VilleV = VilleB
```

18. Les buveurs qui habitent dans la même ville que le buveur 1400 (traiter les deux cas selon que l'on souhaite avoir dans les résultats le buveur 1400).<br />



19. Les commandes qui spécifient une quantité du vin 140 inférieure à celle que spécifie la commande 11 pour ce même vin.<br />
20. Les vins pour lesquels il n'y a pas de commande.
21. Liste des buveurs (num et nom) n'ayany commandé que du Bourgogne (au moins 2 solutions).<br />
22. Liste des buveurs (num et nom) qui ont commandé du Bourgogne et du Bordeau (au moins 2 solutions).
##Requêtes avec Agrégats
23. Afficher pour chaque région son nom et son nombre de vins.<br />
24. Afficher pour chaque viticulteur son nom, son numéro et le nombre de crus qu'il produit.<br />
25. Afficher le nom, le numéro et la quantité moyenne commandée pour chaque buveur de PARIS.<br />
26. Afficher le nombre de commandes par buveur.<br />
27. Total des quantités commandées pour chaque buveur dont la moyenne des quantités commandées est égale ou supérieure à 12.<br />
28. Noms et numéros des viticulteurs qui produisent au moins deux vins de crus différents.<br />
29. Les vins (numéro, cru, nombre de commandes) ayant été commandés au moins deux fois.<br />
30. Liste des commandes non entièrement livrées.<br />

#TP Gestion

1. Donnez la liste des noms d'employés.<br />
```
SELECT ename from EMP ;
```
2. Donnez la liste des noms et date d'embauche du département 20.<br />
```
SELECT ename, hiredate from EMP
WHERE DEPTNO = 20 ;
```
3. Donnez la somme des salaires payés dans l'entreprise.<br />
```
SELECT SUM(SAL) from EMP ;
```
4. Donnez la liste des employés (nom, poste et salaire qui gagnent lus de 2800 et qui sont managers).<br />
```
SELECT ENAME, SAL, JOB from EMP
WHERE JOB = "MANAGER" and SAL > 2800 ;
```
5. Donnez la liste des employés don le départment est situé à Dallas.<br />
```
SELECT ENAME FROM EMP e, DEPT d
WHERE d.DEPTNO = e.DEPTNO AND LOC = "DALLAS";
```
6. Donnez la liste des employés du département 30 (nom, poste et salaire), triée sur le salaire par ordre croissant.<br />
```
SELECT ENAME, JOB, SAL from EMP ORDER BY SAL ASC ;
```
7. Donnez l'ensemble des différents postes de l'entreprise (tous distincts).<br />
```
SELECT DISTINCT JOB from EMP ;
```
8. Donnez la liste des emloyés dont le nom commence par M et dont le salaire est supérieur ou égal à 1290€.<br />
```
SELECT * FROM EMP
WHERE ENAME LIKE "M%"
AND SAL >= 1290 ;
```
9. Donnez la situation du département dans lequel travalle l'employé de nom Allen.<br />
```
SELECT DNAME, LOC from EMP e, DEPT d
WHERE e.DEPTNO = d.DEPTNO AND ENAME= "ALLEN";
```
10. Donner la liste des départements qui possèdent des postes CLERK, SALESMAN ou ANALYST.<br />
```
DESCRIBE DEPT ;
SELECT DISTINCT d.DEPTNO, DNAME from DEPT d, EMP e
WHERE JOB IN ("CLERK", "SALESMAN", "ANALYST")
AND d.DEPTNO = e.DEPTNO ;
```
11. Donner la liste des managers (la valeur associée à sa fonction est MANAGER), dont les subordonnés perçoivent une commission.<br />
```

```
12. Donner la liste des employés dont le département est situé à DALLAS ou CHICAGO, et dont le salaire est supérieur à 1000.<br />
```
SELECT EMPNO, ENAME from EMP e, DEPT d
WHERE LOC IN ("DALLAS", "CHICAGO")
AND SAL > 1000
AND e.DEPTNO = d.DEPTNO ;
```
13. Donner un récapitulatif, qui pour chaque nom de département, donne le nom des postes, la somme des salaires dépensés pour chaque poste, le nombre d'employés sur chaque poste dans ce département. Dans le récapitulatif, ne doivent apparaître que les départments qui ont plus de deux eployés sur un type de poste donné..<br />
```

```
14. Pour chaque employé, donnez le nom, le salaire, l'échelle de salaire, le nom de son supérieur hiéarchique direct, le numéro du département, et la ville du département. La liste doit être ordonnée sur le salaire.<br />
```
SELECT
ORDER BY SAL ASC ;
```
15. Pour chaque département, calculer la moyenne des salaires.<br />
```

```
16. Pour chaque employé, donnez la proportion de la commission et du salaire (par rapport au montant total perçu).<br />
```

```
17. Quels sont les départements qui coûtent le plus cher, et le moins cher en terme de salaire? On donnera le résultat en une seule requête.<br />
```

```
18. Donner la liste des numérs de département qui ont plus de 3 employés, mais qui ne sont pas situés à Chicago.<br />
```

```
19. Donner la liste des numéros de départements qui n'ont pas d'employés.<br />
```

```
20. Quel est le département qui emploie le plus de salariés?.<br />
```

```
