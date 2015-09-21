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
WHERE
heure > 13
```
4. Adresse et ville des patients qui ont consulté tous les médecins de l'hôpital.
```

```
5. Calculez le montant total payé par chaque patient parisien.
```
SELECT SUM(montant)
FROM RDV r, Patient p
WHERE p.numP = r.numP
AND ville = "Paris" ;
```
6. Nom des patients qui ont consulté en 2012 et en 2013.
```
SELECT p.nomP
FROM Patient p, RDV r
WHERE p.numP = r.numP
AND r.date IN(LIKE %2012, %2013)
```
7. Numéro de téléphone des patients qui ont consulté en radiologie ou en orthopédie le 1er janvier 2014.
```
SELECT telP
FROM Patient p, Medecin m, RDV r
WHERE p.numP = r.numP
AND m.numM = r.numM
AND m.spé IN ("radiologie", "orthopédie")
AND date = 01-01-2014 ;
```
8. Donnez le nombre total de rendez-vous pris par chaque patient, uniquement pour les patients ayant pris plus de 5 rendez-vous dans l'hôpital.
```
SELECT COUNT(numRDV)
FROM RDV r, Patient p
WHERE p.numP = r.numP
GROUP BY p.numP
HAVING (COUNT(numRDV) > 5) ;
```
