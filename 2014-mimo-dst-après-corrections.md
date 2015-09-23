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
  )
```

5. Calculez le montant total payé par chaque patient parisien.
```

```
6. Nom des patients qui ont consulté en 2012 et en 2013.
```
```
7. Numéro de téléphone des patients qui ont consulté en radiologie ou en orthopédie le 1er janvier 2014.
```
```
8. Donnez le nombre total de rendez-vous pris par chaque patient, uniquement pour les patients ayant pris plus de 5 rendez-vous dans l'hôpital.
```

```
10. Créer la table RDV(*numRDV*, numP#, numM#, date, heure, montant)
```
```
11. Insérer 1 patient dans Patient(*numP*, nomP, adrP, ville, telP)
```
```
