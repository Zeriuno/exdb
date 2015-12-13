Sujet 1

1.

SELECT /*DISTINCT*/ p.nomP
FROM Patient p, Medecin m, RDV r
WHERE p.numP = r.numP
AND m.numM = r.numM
AND spécialité = "chirurgie"
ORDER BY nomP ASC;

/*DISTINCT permet d'éliminer les consultations multiples mais risque de cacher des homonymes.*/

2.

SELECT DISTINCT ville
FROM Patient p, RDV r
WHERE p.numP = r.numP
AND date NOT LIKE "%-08-2015";
/*il faut faire une différence*/
