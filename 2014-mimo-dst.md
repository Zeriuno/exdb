DST MIMO 2014
Requêtes

Patient(*numP*, nomP, adrP, ville, telP)
Medecin(*numM*, nomM, spé)
RDV(*numRDV*, numP#, numM#, date, heure, montant)

Traduire en SQL les requêtes suivantes.

1. Lister, par ordre alphabétique, les noms des patients qui ont consulté le Dr. Sugar, diabétologue, le 31 mai 2013.
```
SELECT p.nomP
FROM Patient p, RDV r, Medecin m
WHERE p.numP = r.numP
AND m.numM = r.numM
AND m.nomM = "Sugar"
AND spé = "diabétologie"
AND r.date = 31-05-2013
ORDER BY p.nomP ASC ;
```
2. Calculer le coût moyen des RDV en ophtalmologie (tous patients et tous médecins confondus).
```
SELECT AVG(montant)
FROM RDV r, Medecin m
WHERE m.numM = r.numM
AND spé = "ophtalmologie" ;
```
3. Identifiant des médecins qui ne consultent qu'après 13h.
```
SELECT m.numM
FROM Medecin m
WHERE NOT EXISTS ( #différence
  SELECT heure #ou *
  FROM RDV r
  WHERE heure < "13:00"
  AND m.numM = r.numM
  )
```
Syntaxe avec NOT IN par M.me Le Grand
```
SELECT numM
FROM Medecin
WHERE numM NOT IN (
  SELECT numM
  FROM RDV
  WHERE heure < "13:00"
  )
```
↑Ces cas font apparaître les médecins n'ayant aucun rendez-vous. Si on prend les numM de RDV, on obtient des numéros de Médecins ayant des rendez-vous.

4. Adresse et ville des patients qui ont consulté tous les médecins de l'hôpital.
Monter adresse et ville des patients pour lesquels il n'existe pas de médecin pour lequel il n'existe pas de rdv pour ce médecin et pour ce patient (lien rdv-médecin)
```
SELECT p.adresse, p.ville
FROM Patient p
WHERE NOT EXISTS (
  SELECT m.numM
  FROM Medecin m
  WHERE NOT EXISTS (
    SELECT r.*
    FROM RDV r
    WHERE m.numM = r.numR
    AND p.numP = r.numP
    )
  )
```
Adresse et ville des patients ayant consulté tous les chirurgiens
Restrictions concernant les médecins, à mettre donc dans le bloc les concernant.

5. Calculez le montant total payé par chaque patient parisien.
```
SELECT SUM(montant) r.numP
FROM RDV r, Patient p
WHERE p.numP = r.numP
AND ville = "Paris" ;
#correction: il fallait le montant total pour toutes les visites d'un patient, du coup un GROUP BY r.numP dont l'argument doit être aussi dans le SELECT
```
6. Nom des patients qui ont consulté en 2012 et en 2013.
```
SELECT p.nomP
FROM Patient p, RDV r
WHERE p.numP = r.numP
AND r.date IN(LIKE %2012, %2013)
```
1. la date est une chaîne de caractères: LIKE "%2012"
2. On obtient un "ou inclusif"

Ce qu'il faut est une intersection des ensembles de ceux qui ont eu un rendez-vous en 2012 et ceux qui l'ont eu en 2013 = Le numéro des patients doit apparaître dans ces deux sous-ensemles
```
SELECT p.nomP
FROM Patient p, RDV r
WHERE p.numP = r.numP
AND date LIKE "%2012"
AND r.numP IN (
  SELECT r1.numP
  FROM RDV r1
  AND date LIKE "%2013"
  )
```
Possible de faire cela dans une seule requête avec une double jointure avec RDV (deux RDV, l'un pour 2012, l'autre pour 2013).

Avec EXISTS
```
SELECT p.nomP
FROM RDV r, Patient p
WHERE r.numP = p.numP
AND r.date LIKE "%2013"
AND EXISTS (
  SELECT r1.*
  FROM RDV r1
  WHERE r1.date LIKE "%2012"
  AND p.numP = r1.numP
  )
```

7. Numéro de téléphone des patients qui ont consulté en radiologie ou en orthopédie le 1er janvier 2014.
```
SELECT telP
FROM Patient p, Medecin m, RDV r
WHERE p.numP = r.numP
AND m.numM = r.numM
AND m.spé IN ("radiologie", "orthopédie") #ou bien AND (m.spé = "radiologie" OR m.spé = "orthopédie")
AND date = 01-01-2014 ;
```
Le format date est une chaîne, donc entre guillemets
La spécialité alternative pourrait être faite par une sous-requête OR IN
8. Donnez le nombre total de rendez-vous pris par chaque patient, uniquement pour les patients ayant pris plus de 5 rendez-vous dans l'hôpital.
```
SELECT COUNT(numRDV)
FROM RDV r, Patient p
WHERE p.numP = r.numP
GROUP BY p.numP
HAVING (COUNT(numRDV) > 5) ;
```
Ce qui est dans le GROUP BY doit être dans le SELECT
10. Créer la table RDV(*numRDV*, numP#, numM#, date, heure, montant)
```
CREATE TABLE RDV (
  numRDV integer Primary key,
  numP integer(8) not null #integer() à vérifier
  numM integer(8) not null
  date date
  heure time
  montant float  
  CONSTRAINT
    FOREIGN KEY (numP) references Patient (numP)
    FOREIGN KEY (numM) references Medecin (numM)
  );
```
11. Insérer 1 patient dans Patient(*numP*, nomP, adrP, ville, telP)
```
INSERT INTO Patient
  VALUES (01, "Réno", "4 rue Victor Cousin", "Paris", 0115181790);
```
