DST MIMO 2014
Requêtes

Patient(*numP*, nomP, adrP, ville, telP)
Medecin(*numM*, nomM, spé)
RDV(*numRDV*, numP#, numM#, date, heure, montant)

Traduire en SQL les requêtes suivantes.

1. Lister, par ordre alphabétique, les noms des patients qui ont consulté le Dr. Sugar, diabétologue, le 31 mai 2013. → restriction simple avec trois tables
```
```
2. Calculer le coût moyen des RDV en ophtalmologie (tous patients et tous médecins confondus).
→ table Medecin (attribut spé), table RDV et AVG

```
```
3. Identifiant des médecins qui ne consultent qu'après 13h.
→ Medecin, RDV. Différence: ceux qui consultent, moins ceux qui consultent avant.
Leur numéro de médecin n'est pas dans la liste des RDV avant 13 heures.

```
```
4. Adresse et ville des patients qui ont consulté tous les médecins de l'hôpital.
TOUS les médecins, objet des patients → division. Pour obtenir la syntaxe de la requête, tourner cela par deux "il n'y a pas".

Patients pour lesquels il n'y a pas de médecins (table de l'objet) pour lesquels il n'y a pas de rendez-vous (table qui les lie) pour ce médecin pour ce patient (conditions de jointure)
```
```
5. Calculez le montant total payé par chaque patient parisien.

→ RDV (montant) + Patient (ville, qui doit être spécifiée). Comme on demande "pour chaque patient", il faut regrouper par numéro de patient, et l'argument du GROUP BY doit être dans le SELECT
```
```
6. Nom des patients qui ont consulté en 2012 et en 2013.
→ Une double condition à satisfaire pour le même attribut (la date du rendez-vous): soit requête imbriquée, soit jointure
```
```
7. Numéro de téléphone des patients qui ont consulté en radiologie ou en orthopédie le 1er janvier 2014.
→ Patient, Médecin (spé), RDV (date).
```
```
8. Donnez le nombre total de rendez-vous pris par chaque patient, uniquement pour les patients ayant pris plus de 5 rendez-vous dans l'hôpital.
→ RDV, Patient. Chaque → GROUP BY (dont l'argument va dans le SELECT). Ayant pris: restriction sur les groupes, HAVING.
```
```
10. Créer la table RDV(*numRDV*, numP#, numM#, date, heure, montant)
```
```
11. Insérer 1 patient dans Patient(*numP*, nomP, adrP, ville, telP)
```
```
