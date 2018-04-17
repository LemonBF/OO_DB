#### Participants 
###### Eric, Victor, Alex 

-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
## Introducció
## Fem insercions, updates i deletes de la colecció "taxistes"



```sql

-- Insertem a un nou taxista a la colecció taxistes amb el numero de cotxe 3030

INSERT INTO TABLE (SELECT i.INFO_TAXISTA
		   FROM info_taxi i 
		   WHERE i.NUM_COCHE = 3030)
	VALUES (550, 'Pedro', 5558555555, '@pepet');

SELECT T.NUM_COCHE, X.* FROM info_taxi t, TABLE(t.INFO_TAXISTA) X

NUM_COCHE |PLACA |NOMBRE |TELF       |MAIL   |
----------|------|-------|-----------|-------|
3030      |30    |Pepet  |333333333  |@      |
3030      |550   |Pedro  |5558555555 |@pepet |

-- Agafem un taxista amb el numero de placa 550 i modifiquem les seves variables

UPDATE TABLE(SELECT i.info_taxista
             FROM info_taxi i
             WHERE i.NUM_COCHE = 3030) e   
   SET VALUE(e) = taxista(10, 'Pedro', 999666333, '@pedro')
   WHERE e.placa = 550;

SELECT T.NUM_COCHE, X.* FROM info_taxi t, TABLE(t.INFO_TAXISTA) X


NUM_COCHE |PLACA |NOMBRE |TELF      |MAIL   |
----------|------|-------|----------|-------|
3030      |30    |Pepet  |333333333 |@      |
3030      |10    |Pedro  |999666333 |@pedro |


-- Eliminem el taxista mitjançant el seu numero de placa de la colecció "taxistes" 

DELETE FROM TABLE(SELECT i.info_taxista
                  FROM info_taxi i
                  WHERE i.NUM_COCHE = 3030) e
   WHERE e.placa = 10;

SELECT T.NUM_COCHE, X.* FROM info_taxi t, TABLE(t.INFO_TAXISTA) X


NUM_COCHE |PLACA |NOMBRE |TELF      |MAIL |
----------|------|-------|----------|-----|
3030      |30    |Pepet  |333333333 |@    |
