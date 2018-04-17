# Authors: Oriol Julià Brunet i Jordi Besalú Colomer

## Introducció
  En la nostra BBDD creem un objecte gos amb les seves propietats, tot seguit creem la col·lecció races_gos.
  Finalment creem la taula de col·leccións on es mostra la informació de gos a partir de la seva raça.

```sql
-- Creem objecte gos amb les seves propietats
CREATE TYPE gos AS OBJECT (
    id_gos VARCHAR2(20),
    nom VARCHAR2(30),
    color VARCHAR2(10),
    pes VARCHAR2(10)
);

-- Creem la col·lecció races_gos
CREATE TYPE races_gos AS TABLE OF gos;

--Creem la taula de col·leccións gossera.
CREATE TABLE gossera (   
    id_raca VARCHAR2(20),
    nom_raca VARCHAR2(10),
    gos_raca races_gos)
    NESTED TABLE gos_raca STORE AS gossera_nested;

--Insertem les dades.
INSERT INTO gossera VALUES(
    '1', 'Husky',
    races_gos(gos('00', 'Rex', 'Gris', '10'),
              gos('01', 'Balto', 'Blanc', '12'))
);

INSERT INTO gossera VALUES(
    '1', 'PastorA',
    races_gos(gos('02', 'Thor', 'Marro', '6'),
              gos('03', 'Firulais', 'Negre', '8'))
);

--Fem un select de totes les raçes amb els seus gossos.
SELECT * FROM GOSSERA g, TABLE(g.gos_raca) 
```
#### Resultat de la select:

ID_RACA |NOM_RACA |GOS_RACA          |ID_GOS |NOM      |COLOR |PES |
--------|---------|------------------|-------|---------|------|----|
0       |Husky    |['O.GOS','O.GOS'] |00     |Rex      |Gris  |10  |
0       |Husky    |['O.GOS','O.GOS'] |01     |Balto    |Blanc |12  |
1       |PastorA  |['O.GOS','O.GOS'] |02     |Thor     |Marro |6   |
1       |PastorA  |['O.GOS','O.GOS'] |03     |Firulais |Negre |8   |


```sql
--Fem un update.
UPDATE gossera SET gos_raca =
  races_gos(gos ('02', 'Odin', 'Marro', '8'),
            gos('03', 'Mike', 'Marro', '9'))
WHERE id_raca = 1

--Fem un altre cop la select.
SELECT * FROM GOSSERA g, TABLE(g.gos_raca) 

```
#### Resultat select:
ID_RACA |NOM_RACA |GOS_RACA          |ID_GOS |NOM   |COLOR |PES |
--------|---------|------------------|-------|------|------|----|
0       |Husky    |['O.GOS','O.GOS'] |00     |Rex   |Gris  |10  |
0       |Husky    |['O.GOS','O.GOS'] |01     |Balto |Blanc |12  |
1       |PastorA  |['O.GOS','O.GOS'] |02     |Odin  |Marro |8   |
1       |PastorA  |['O.GOS','O.GOS'] |03     |Mike  |Marro |9   |
