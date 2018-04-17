## Autores:
  - Dmytro Holota
  - Cristian Moraru
  - Hamza Sadouki
## Objetivos de la practica:
  Modificacion, inserción y eliminacion de objetos en una coleccion 

## Practica: 
```SQL
--Modificar element de un objeto en una coleccion
begin
  update table(select alumnes_column from daw2 where group_no = 1)
  set cognoms = 'Pistacho'
  where nom = 'Cristian';
end;

select * from daw2 d,table(d.alumnes_column);

1	{{40,Cristian,Pistacho,Mail},{20,Dani,Herrera,MAIL}}	40	Cristian	Pistacho	Mail
1	{{40,Cristian,Pistacho,Mail},{20,Dani,Herrera,MAIL}}	20	Dani	Herrera	MAIL


--Eliminar objeto de una collecion
begin
  delete from table(select alumnes_column from daw2)
  where nom = 'Cristian';
end;




select * from daw2 d,table(d.alumnes_column);

1	{{20,Dani,Herrera,MAIL}}	20	Dani	Herrera	MAIL

--Insertar un objeto dentro de una coleccion
begin
  insert into table(select alumnes_column from daw2)
  values (40,'Cristian','MuyRaro','Mail');
end;

select * from daw2 d,table(d.alumnes_column);

1	{{40,Cristian,MuyRaro,Mail},{20,Dani,Herrera,MAIL}}	40	Cristian	MuyRaro	Mail
1	{{40,Cristian,MuyRaro,Mail},{20,Dani,Herrera,MAIL}}	20	Dani	Herrera	MAIL

```
