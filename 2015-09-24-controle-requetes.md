1.

CREATE TABLE Vetement (
  nVet integer PRIMARY KEY,
  type varchar[50] not null,
  couleur varchar[30],
  taille varchar[10],
  prix float);


2.



3.

INSERT INTO Vetement VALUES (
  1, "pantalon", "noir", "38", 40,75);

4.


5.


6.

SELECT adr, ville
FROM Boutique b
WHERE b.nBout NOT IN
(
  SELECT b.nBout
  FROM Boutique b, Achat a, Vetement v
  WHERE b.nBout = a.nBout AND a.nVet = v.nVet
  AND taille <> "XL");
