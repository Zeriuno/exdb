DST MIMO 2013, 9 Décembre 2013.

Événement (id_even*, nom, date, tarif, id_org#, id_lieu#)
Organisme(id_org*, nom, type, adresse)
Lieu(id_lieu*, nom, type, adresse)
Intervenant(id_interv*, nom, prenom, coordonnées, id_org#)
Feedback_visiteur(id_fb*, texte, id_even#)
Intervenir([id_even#,id_interv#]*, nb_heures)

1. Liste des événements qui se sont déroulés entre le 1er juillet 2013 et le 31 août 2013.
→Simple restriction temporelle:
* BETWEEN
* date > 01-07-2012
  AND date < 31-08-2013
* AND (LIKE "%-07-2013" OR LIKE "%-08-2013")
```
SELECT e.*
FROM Événement e
WHERE
```
2. Nom de tous les musées qui ont été le lieu d'un événement.
```

```
3. Nom des intervenants qui sont intervenus en 2012 et en 2013.

Double condition pour un même champs: le nom doit satisfaire la première et être dans la liste de ceux qui sont dans le deuxième cas.
```

```
4. Liste des événements (en indiquant le nom de l'organisateur et du lieu, et non leur identifiant), triée par ordre chronologique inverse.
date DESC
```

```
5. Tarifs minimum, moyen et maximum des événements.
MIN, AVG, MAX
```

```
6. Nombre de lieux différents où se sont déroulés des événements.
DISTINCT
```

```
7. Identifiant, nom, prénom, et nom d'organisme des intervenants qui ne sont intervenus dans aucun événement.
leur identifiant NOT IN l'autre table
```

```
8. Nom et prénom des identifiants qui :
  * Soit font partie d'un organisme qui a organisé au moins un événement
  * Soit sont intervenus sur au moins un événement pendant plus de 6 heures.

  AND (
    intervenant, org, evenement
    OR IN (
      intervenant, evenement, intervenir
      where heures >
      6

    )
```

```
9. Par événement (indiquer le nom des événements), donner l'identifiant, le nom, prénom et nombre d'interventions des intervenants dont le nombre d'heures d'intervention total est supérieur ou égal à 20 heures.
```

```
10. Nom du ou des lieux les plus utilisés pour des événements.
```

```
11. Nom des organismes qu interviennent dans tous les événements, qu'ils soient ou non organisateurs. (NB: il y a toujours des intervenants d'un organisme organisateur d'un événement qui interviennent lors de cet événement).
```

```
Ajout 2015:
12. Création de toutes les tables, dans le bon ordre.
```

```
13. Insertion de données dans toutes les tables.
```

``` 
