```SQL
--Modificar element de un objeto en una coleccion
begin
  update table(select alumnes_column from daw2)
  set cognoms = 'supoku'
  where nom = 'Hamza';
end;

--Eliminar objeto de una collecion
begin
  delete from table(select alumnes_column from daw2)
  where nom = 'Cristian';
end;

--Insertar un objeto dentro de una coleccion
begin
  insert into table(select alumnes_column from daw2)
  values (30,'Cristian','MuyRaro','Mail');
end;
```
