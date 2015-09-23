DST MIMO 2014
Requêtes

Patient(*numP*, nomP, adrP, ville, telP)
Medecin(*numM*, nomM, spé)
RDV(*numRDV*, numP#, numM#, date, heure, montant)

Traduire en SQL les requêtes suivantes.

1. Lister, par ordre alphabétique, les noms des patients qui ont consulté le Dr. Sugar, diabétologue, le 31 mai 2013.
```
SELECT nomP
from Patient p, RDV r, Medecin m
WHERE nomM = "Sugar"
AND spé ="diabétologie"
AND date = "31-05-2013"
AND m.numM = r.numM
AND p.numP = r.numP
ORDER BY nomP ASC ;
```
ASC est l'ordre par défaut, le cas inverse est DESC

2. Calculer le coût moyen des RDV en ophtalmologie (tous patients et tous médecins confondus).
```
SELECT AVG(montant)
FROM RDV r, Medecin m
WHERE r.numM = m.numM
AND spé = "ophtalmologie" ;
```
3. Identifiant des médecins qui ne consultent qu'après 13h.
```
SELECT r.numM
FROM RDV r,
WHERE r.numM NOT IN (
  SELECT r.numM
  FROM RDV r
  WHERE heure <= "13:00" ;
  )
```
4. Adresse et ville des patients qui ont consulté tous les médecins de l'hôpital.

Les patients pour lesquels il n'y a pas de médecins pour lesquels il n'y a pas de visites pour ce médecin pour ce patient
```
SELECT adresse, ville
FROM Patient p
WHERE NOT EXISTS (
  SELECT *
  FROM Medecin m
  WHERE NOT EXIST (
    SELECT *
    FROM RDV r
    WHERE r.numM = m.numM
    AND p.numP = r.numP
    )
  ) ;
```

5. Calculez le montant total payé par chaque patient parisien.
```
SELECT SUM(montant), r.numP
FROM RDV r, Patient p
WHERE ville = "Paris"
GROUP BY r.numP ;
```
6. Nom des patients qui ont consulté en 2012 et en 2013.
```
SELECT nomP
FROM Patient p, RDV r
WHERE p.numP = r.numP
AND date LIKE "%2012"
AND r.numP IN (
  SELECT r.numP
  FROM RDV
  AND date LIKE "%2013"
  ) ;
```
```
SELECT nomP
FROM Patient p, RDV r
WHERE p.numP = r.numP
AND date LIKE "%2012"
AND EXISTS (
  SELECT *
  FROM RDV r1
  AND r1.date LIKE "%2013"
  AND r.numP = r1.numP
  ) ;
```
7. Numéro de téléphone des patients qui ont consulté en radiologie ou en orthopédie le 1er janvier 2014.
```
SELECT telP
FROM Patient p, RDV r, Medecin m
WHERE r.numP = p.numP
AND r.numM = m.numM
AND (spé = "radiologie"
OR spé = "orthopédie")
AND date = "01-01-2014"
```
```
SELECT telP
FROM Patient p, Medecin m, RDV r
WHERE p.numP = r.numP
AND m.numM = r.numM
AND spé IN ("radiologie", "othopédie")
AND date = "01-01-2014"
```
8. Donnez le nombre total de rendez-vous pris par chaque patient, uniquement pour les patients ayant pris plus de 5 rendez-vous dans l'hôpital.
```
SELECT COUNT(r.numRDV)
FROM RDV r, Patient p
WHERE p.numP = r.RDV
GROUP BY r.numP
HAVING (COUNT(r.numRDV)) > 5
```
10. Créer la table RDV(*numRDV*, numP#, numM#, date, heure, montant)
```
CREATE TABLE RDV (
  numRDV integer Primary key,
  numP integer not null,
  numM integer not null,
  date date,
  heure time,
  montant float,
  CONSTRAINT
    FOREIGN KEY (numP) references Patient (numP)
    FOREIGN KEY (numM) references Medecin (numM)
  )
```
11. Insérer 1 patient dans Patient(*numP*, nomP, adrP, ville, telP)
```
INSERT INTO Patient
  VALUES (007, 'BOND', '21b Baker Street', 'London', NULL)
```
