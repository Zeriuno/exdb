#TP Monuments

/*Sauf mention contraire, le code est de M. du Mouza, tous droits réservés*/

/*Pour obtenir le jour de la semaine correspondant à une date*/
select to_char(to_date('27/04/1984', 'DD/MM/YYYY'), 'DAY') FROM dual;


CREATE OR REPLACE FUNCTION gain(p_ Monument.destination%TYPE) RETURN INTEGER IS
  v_gain Visite.prix%TYPE;
BEGIN
  SELECT SUM(prix) INTO v_gain
  FROM Visite
  WHERE Visite.destination = p_;
  RETURN v_gain;
END;
/

CREATE OR REPLACE PROCEDURE gain_pays AS
CURSOR total_pays IS
  SELECT pays, SUM(prix) as total
  FROM Visite, Monument
  WHERE Monument.destination=Visite.destination
  GROUP BY pays;
v_total_pay total_pays%ROWTYPE;
BEGIN
  OPEN total_pays;
  FETCH total_pays INTO v_total_pay;
  WHEN (total_pays%FOUND)
  LOOP
    DBMS_OUTPUT.PUTLINE.PUT_LINE (v_total_pays.pays || ' a rapporté '!! v_total_pays.total);
    FETCH total_pays into v_total_pays;
  END LOOP;
  CLOSE total_pays;
END;
/


##Trigger

1. Créer un trigger qui empêche d'insérer un monument dont la date est postérieure à la date actuelle

CREATE OR REPLACE TRIGGER no_future
BEFORE /*ou AFTER*/ INSERT OR UPDATE on Monument
FOR EACH ROW
DECLARE
  this_year NUMBER(4);
BEGIN
  this_year := to_char(sysdate, 'YYYY');
  IF(this_year < :NEW.an) THEN
   RAISE_APPLICATION_ERROR(-20010, 'Retour vers le futur!');
  END IF;
END;
/

INSERT INTO Monument values('YOLO','YO','LO',2077);
