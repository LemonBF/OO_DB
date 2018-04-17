CREATE TYPE taxista AS OBJECT (
	placa NUMBER,
	nombre VARCHAR2(30),
	telf NUMBER,
	mail VARCHAR2(30),
	disponible CHAR(1)
);

CREATE TYPE taxistas AS TABLE OF taxista;



CREATE OR REPLACE TYPE flota AS OBJECT (
num_flota VARCHAR2(10),
flota_taxistas taxistas,
MEMBER FUNCTION getLibre RETURN taxista);



CREATE OR REPLACE TYPE BODY flota AS
MEMBER FUNCTION getLibre RETURN taxista IS
BEGIN
   FOR i in 1..SELF.flota_taxistas.COUNT LOOP
         IF (flota_taxistas(i).disponible = 'Y')
           THEN
             RETURN SELF.flota_taxistas(i);
         END IF;
      END LOOP; 
 END;
END;

CREATE TABLE empresa_taxi OF flota
NESTED TABLE flota_taxistas STORE AS flota_taxistas_nt;


INSERT INTO empresa_taxi
VALUES(
flota(2526,
    taxistas(taxista(002, 'John Price', 2563256852,'@','Y'), 
             taxista(003, 'John Soap', 589654758,'@','Y'),
             taxista(004, 'Adam Sandler', 558725986,'@','N'),
             taxista(006, 'Derek Westbrook', 236589147,'@', 'Y'))));


INSERT INTO empresa_taxi
VALUES(
flota(3685,
	taxistas(taxista(012, 'John Price', 2525656852,'@','N'), 
             taxista(013, 'John Soap', 589651111,'@','N'),
             taxista(014, 'Adam Sandler', 222225986,'@','N'),
             taxista(016, 'Derek Westbrook', 236523547,'@', 'Y'))));
             
DECLARE
	flota_taxis flota;
	disponible taxista;
BEGIN
  SELECT deref(ref(e)) INTO flota_taxis FROM empresa_taxi e WHERE e.num_flota = '2526';
  disponible := flota_taxis.getLibre();
  DBMS_OUTPUT.PUT_LINE(disponible.nombre);
END;