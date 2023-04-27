## Con el fichero "libros.xml"

1. Títulos ordenados.

```
for $x in doc("libros.xml")/libros/libro
return $x/titulo 
```

2. Títulos ordenados por el nombre del primer autor.

```
for $x in doc("libros.xml")/libros/libro
order by head($x/autores/autor/nombre)
return $x/titulo
```

3. Nombre y apellido del primer autor de cada libro.

```
for $x in doc("libros.xml")/libros/libro/autores
return head($x/autor/nombre) | head($x/autor/apellido)
```

4. Título y número de autores de cada libro.

```
for $x in doc("libros.xml")/libros/libro
return <response>{data($x/titulo)} tiene {count($x/autores/autor)} autor/es</response>
```

5. Libros que tienen varios autores y libros que tienen un autor

```
for $x in doc("libros.xml")/libros/libro
where count($x/autores/autor)>1
return $x/titulo
```
```
for $x in doc("libros.xml")/libros/libro
where count($x/autores/autor)=1
return $x/titulo
```

6. Libros cuyo autor es “Axel”.

```
for $x in doc("libros.xml")/libros/libro
where some $a in $x/autores/autor/nombre satisfies ($a="Axel")
return $x/titulo
```

7. Mostrar título y precio en una lista HTML.

```
<ul>
{
for $x in doc("libros.xml")/libros/libro
return <li>{data($x/titulo)}: {data($x/precio)} €</li>
}
</ul>
```

8. Mostrar: título, isbn y precio en una tabla HTML

```
<table>
  <tr>
    <th>Título</th>
    <th>isbn</th>
    <th>precio</th>
  </tr>
  <tr>
    {
    for $x in doc("libros.xml")/libros/libro
    return (<td>{data($x/titulo)}</td>,<td>{data($x/isbn)}</td>,<td>{data($x/precio)}</td>)
    }
  </tr>
</table>
```


## Con el fichero "alumnos.xml"

1. Mostrar el nombre de los alumnos aprobados.

```
for $x in doc("alumnos.xml")/alumnos/alumno
where $x/nota>5
return $x/nombre
```

2. Mostrar el DNI y la nota de los alumnos que han aprobado.

```
for $x in doc("alumnos.xml")/alumnos/alumno
where $x/nota>5
return <aprobados>El alumno con DNI {data($x/@dni)} ha aprobado con un {data($x/nota)}</aprobados>
```

3. Indicar el nombre de los alumnos cuyas notas están entre 6 y 8 (ambas inclusive).

```
for $x in doc("alumnos.xml")/alumnos/alumno
where $x/nota>=6 and $x/nota<=8
return $x/nombre
```

4. Mostrar listado de nombres ordenados por apellidos.

```
for $x in doc("alumnos.xml")/alumnos/alumno
order by $x/apells
return $x/nombre
```

5. Mostrar nombres ordenados por DNI.

```
for $x in doc("alumnos.xml")/alumnos/alumno
order by $x/@dni
return $x/nombre
```

6. Mostrar el Nº de cada alumno y su nombre.

```
for $x at $i in doc("alumnos.xml")/alumnos/alumno
return <alumno>{$i}. {data($x/nombre)}</alumno>
```
