## Autores:
  - Dmytro Holota
  - Hamza Sodouki
  - Cristian Moraru
## Objetivo de la practica:
   -Treballar amb col·leccions d'objectes.
   -Invocar mètodes de l'objecte.
## Practica: 
```SQL 
-- Creamos el objeto alumne
create type alumne_tip as object (
  idno int ,
  nom varchar2(20),
  cognom varchar2(20),
  delegat int
);
-- creamos una colecciones de objetos alumne
create type alumnes_tip as table of alumne_tip;
-- creamos una tabla clase que contendra una coleccion de alumnes
create or replace type classe_tip as object (
  idno int,
  aula int,
  alumnes alumnes_tip,
  member function getDelegat return alumne_tip
);
/
create or replace type body classe_tip as
--Creamos la funcion que nos devolvera al delegado
member function getDelegat return alumne_tip is
 begin
   for i in 1..SELF.alumnes.COUNT loop
     if (alumnes(i).delegat = 1)
       then
          return SELF.alumnes(i);
     end if;
   end loop;
 end;
end;

-- Creamos la tabla classes que contiene la coleccion de alumnos
create table classes of classe_tip
nested table alumnes STORE AS alumnes_nt;

--Inserts en la tabla "classes"
insert into classes
values(
    classe_tip(1,404,alumnes_tip(
        alumne_tip(1,'Dmytro','Holota',1),
        alumne_tip(2,'Hamza','Sinjuku',0),
        alumne_tip(3,'Cristian', 'MuyRaro',0)
    ))
);

insert into classes
values(
    classe_tip(2,501,alumnes_tip(
        alumne_tip(1,'Paco','Sus',0),
        alumne_tip(2,'TiaBuena','ConCara',1),
        alumne_tip(3,'UnGato', 'Alfredo',0)
    ))
);

-- Ejecutamos la funcion "getDelegat"
declare
 classe classe_tip;
 delegat alumne_tip;
begin
  select value(e) into classe from classes e where e.idno = 1;
  delegat := classe.getDelegat();
  DBMS_OUTPUT.PUT_LINE(delegat.nom);
end;

-- Resultado si el id = 1:
  -- Dmytro
```
