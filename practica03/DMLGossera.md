# Authors Oriol Julià Brunet i Jordi Besalú Colomer

## Introducció
En aquesta pràctica fem Update, Insert i Delete per tal de modificar les nostres col·leccións.

```sql
--Modificar un element d'un objecte de una col·lecció.
BEGIN
    UPDATE TABLE (SELECT gos_raca FROM gossera WHERE id_raca = 0)
    SET color = 'Negre'
    WHERE nom = 'Rex';
END;

SELECT * FROM GOSSERA g, TABLE(g.gos_raca)
```
#### Resultat de la select
ID_RACA |NOM_RACA |GOS_RACA          |ID_GOS |NOM   |COLOR |PES |
--------|---------|------------------|-------|------|------|----|
0       |Husky    |['O.GOS','O.GOS'] |00     |Rex   |Negre |10  |
0       |Husky    |['O.GOS','O.GOS'] |01     |Balto |Blanc |12  |
1       |PastorA  |['O.GOS','O.GOS'] |02     |Odin  |Marro |8   |
1       |PastorA  |['O.GOS','O.GOS'] |03     |Mike  |Marro |9   |

```sql
-- Insertar un nou gos a la col·lecció gossera amb la id de la raça 1
INSERT INTO TABLE (SELECT g.gos_raca FROM gossera g WHERE g.id_raca = '1' )
VALUES ('04', 'Salchi', 'negre', '7');

SELECT * FROM GOSSERA g, TABLE(g.gos_raca)
```
#### Resultat de la select
ID_RACA |NOM_RACA |GOS_RACA                  |ID_GOS |NOM    |COLOR |PES |
--------|---------|--------------------------|-------|-------|------|----|
0       |Husky    |['O.GOS','O.GOS']         |00     |Rex    |Negre |10  |
0       |Husky    |['O.GOS','O.GOS']         |01     |Balto  |Blanc |12  |
1       |PastorA  |['O.GOS','O.GOS','O.GOS'] |02     |Odin   |Marro |8   |
1       |PastorA  |['O.GOS','O.GOS','O.GOS'] |03     |Mike   |Marro |9   |
1       |PastorA  |['O.GOS','O.GOS','O.GOS'] |04     |Salchi |negre |7   |


```sql
--Eliminem el gos que acabem d'insertar amb el nom Salchi
DELETE FROM TABLE (SELECT g.gos_raca FROM gossera g WHERE g.id_raca = 1) o
WHERE o.nom = 'Salchi';

SELECT * FROM GOSSERA g, TABLE(g.gos_raca)
```
#### Resultat de la select
ID_RACA |NOM_RACA |GOS_RACA          |ID_GOS |NOM   |COLOR |PES |
--------|---------|------------------|-------|------|------|----|
0       |Husky    |['O.GOS','O.GOS'] |00     |Rex   |Negre |10  |
0       |Husky    |['O.GOS','O.GOS'] |01     |Balto |Blanc |12  |
1       |PastorA  |['O.GOS','O.GOS'] |02     |Odin  |Marro |8   |
1       |PastorA  |['O.GOS','O.GOS'] |03     |Mike  |Marro |9   |
