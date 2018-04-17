#### Participants 
###### Eric, Victor, Alex 

-------------------------------------------------------------------------------
-------------------------------------------------------------------------------
#### Introducció
##### Base de dades OO de taxistas. Disposem d'un objecte taxista amb propietats : 
* Placa
* Nombre
* Telefono
* Mail
##### I la col·lecció 'Taxistas', on recopilarem tota la informacio dels taxistes.
##### Emmagatzem els taxistes en una taula de col·leccions (info_taxi).
##### Fem insercions, updates i deletes de la colecció "taxistes"

```sql
-- Creamos la coleccion de taxistas

CREATE TYPE taxistas AS TABLE OF taxista;

-- Creamos la tabla de colecciones donde se muestra
-- la información del taxista en base al numero del coche

CREATE TABLE info_taxi (
	num_coche NUMBER,
	info_taxista taxistas)
	NESTED TABLE info_taxista STORE AS info_taxista_nt;

-- Insertamos diferentes taxistas en la colección dependiendo 
-- del coche con el que trabajan

INSERT INTO info_taxi VALUES(
	3030,
	taxistas(taxista(30, 'Pepet', 333333333, '@'))
);

SELECT T.NUM_COCHE, X.* FROM info_taxi t, TABLE(t.INFO_TAXISTA) X

NUM_COCHE |PLACA |NOMBRE |TELF       |MAIL   |
----------|------|-------|-----------|-------|
3030      |30    |Pepet  |333333333  |@      | 


-- Insertem a un nou taxista a la colecció taxistes amb el numero de cotxe 3030

INSERT INTO TABLE (SELECT i.INFO_TAXISTA
		   FROM info_taxi i 
		   WHERE i.NUM_COCHE = 3030)
	VALUES (550, 'Pedro', 5558555555, '@pepet');

SELECT T.NUM_COCHE, X.* FROM info_taxi t, TABLE(t.INFO_TAXISTA) X

NUM_COCHE |PLACA |NOMBRE |TELF       |MAIL   |
----------|------|-------|-----------|-------|
3030      |30    |Pepet  |333333333  |@      |
3030      |550   |Pedro  |5558555555 |@pepet | -- <-- Taxista Pedro afegit  

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
3030      |10    |Pedro  |999666333 |@pedro | -- <-- Modificat variables de taxista Pedro  


-- Eliminem el taxista mitjançant el seu numero de placa de la colecció "taxistes" 

DELETE FROM TABLE(SELECT i.info_taxista
                  FROM info_taxi i
                  WHERE i.NUM_COCHE = 3030) e
   WHERE e.placa = 10;

SELECT T.NUM_COCHE, X.* FROM info_taxi t, TABLE(t.INFO_TAXISTA) X


NUM_COCHE |PLACA |NOMBRE |TELF      |MAIL |
----------|------|-------|----------|-----|
3030      |30    |Pepet  |333333333 |@    |
					    -- <-- Eliminat taxista Pedro
