## Autores:
  - Dmytro Holota
  - Hamza Sodouki
  - Cristian Moraru
## Objetivos de la practica:
  Añadir una función que devuelve al delegado de clase y al final comprar los resultados de las dos funciones.

## Practica:
```SQL
-- Creamos el objeto alumne
create  type alumne_tip as object (
  idno int ,
  nom varchar2(20),
  cognom varchar2(20),
  delegat int,
  MAP MEMBER FUNCTION get_idno RETURN NUMBER
);

CREATE TYPE BODY alumne_tip AS
  MAP MEMBER FUNCTION get_idno RETURN NUMBER IS
  BEGIN
    RETURN idno;
  END;
end;

-- creamos una colecciones de objetos alumne
create type alumnes_tip as table of alumne_tip;

-- creamos una tabla clase que contendra una coleccion de alumnes
create or replace type classe_tip as object (
  idno int,
  aula int,
  alumnes alumnes_tip,
  member function getDelegat return alumne_tip,
  member function getDelegatSelect return alumne_tip
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
-- La misma funcion pero utilizando "select"
member function getDelegatSelect return alumne_tip is
  begin
    declare
      alumne alumne_tip;
      begin
        select value(al) into alumne from table(self.alumnes) al where al.delegat = 1;
        return alumne;
      end;
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

-- Comprobamos los resultados de las dos funciones.
declare
 classe classe_tip;
 alumneMetohod1 alumne_tip;
 alumneMetohod2 alumne_tip;
begin
  select value(e) into classe from classes e where e.idno = 1;
  alumneMetohod1 := classe.getDelegatSelect();
  alumneMetohod2 := classe.getDelegat();
  DBMS_OUTPUT.PUT_LINE(alumneMetohod1.nom);
  DBMS_OUTPUT.PUT_LINE(alumneMetohod2.nom);
  if (alumneMetohod1 = alumneMetohod2)
    then
     DBMS_OUTPUT.PUT_LINE('son iguals');
    else
     DBMS_OUTPUT.PUT_LINE('son diferents');
  end if;
end;
```
