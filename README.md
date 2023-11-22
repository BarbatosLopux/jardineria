# JARDINERIA
A partir de una base de datos brindada se generaran unas consultas pertinentes, a continuacion se presenta la el modelo fisico junto con sus consultas. 

## Modelo Fisico
![imagenModeloFisico](./Imagenes/modeloFisicoJardineria.png)

## Primer listado de consultas 

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```sql
    SELECT c.nombre_cliente AS Nombre_cliente, e.nombre AS Nombre_rep_ventas, e.apellido1 AS Apellido_rep_ventas FROM cliente c INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
```

```sql
    SELECT c.nombre_cliente AS Nombre_cliente,e.nombre AS Nombre_rep_ventas,e.apellido1 AS Apellido_rep_ventas FROM cliente c,empleado e WHERE c.codigo_empleado_rep_ventas = e.codigo_empleado;

```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
    SELECT DISTINCT c.codigo_cliente AS COD_Cliente, c.nombre_cliente AS Nombre_cliente, CONCAT( e.nombre,' ',e.apellido1,' ',e.apellido2) AS Representante_ventas FROM cliente c INNER JOIN empleado e  ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN pago p ON c.codigo_cliente = p.codigo_cliente ;

```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
    SELECT c.codigo_cliente, c.nombre_cliente ,e.codigo_empleado, CONCAT(e.nombre,' ',e.apellido1,' ',e.apellido2 ) AS REPRESENTANTE_VENTAS FROM cliente c LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado WHERE p.codigo_cliente IS NULL;
```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
     SELECT DISTINCT c.codigo_cliente , c.nombre_cliente AS Cliente , e.codigo_empleado, CONCAT (e.nombre,' ',e.apellido1) AS empleado , o.ciudad AS ciudad_Representante_Ventas  FROM cliente c INNER
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN pago p ON c.codigo_cliente = p.codigo_cliente INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
    SELECT c.nombre_cliente , CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2)AS representante_ventas , o.ciudad FROM cliente c LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina WHERE p.codigo_cliente IS NULL;
```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

```sql
     SELECT DISTINCT o.linea_direccion1 AS OFICINA_DIRECCIÓN FROM oficina o INNER JOIN empleado e ON o.codigo_oficina = e.codigo_oficina INNER JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas WHERE c.ciudad = 'Fuenlabrada';
```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
    SELECT c.nombre_cliente , CONCAT (e.nombre,' ',e.apellido1,' ',e.apellido2)AS representante_ventas , o.ciudad AS Ciudad_Representante FROM cliente c INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina ;
```

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

```sql
    SELECT e.nombre AS EMPLEADO, ej.nombre AS JEFE FROM empleado e LEFT JOIN empleado ej ON e.codigo_jefe = ej.codigo_empleado;
```

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

```sql
    SELECT e.nombre AS EMPLEADO, ej.nombre AS JEFE ,ejj.nombre AS JEFE_DEL_JEFE FROM empleado e LEFT JOIN empleado ej ON e.codigo_jefe = ej.codigo_empleado LEFT JOIN empleado ejj ON ej.codigo_jefe = ejj.codigo_empleado;
```

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

```sql
    /* Agregando la entrega de fechas como NULL*/

    SELECT DISTINCT c.nombre_cliente FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE p.fecha_entrega > p.fecha_esperada;
    
    /* Entrega de fechas como NULL  */
    SELECT DISTINCT c.nombre_cliente, p.fecha_esperada, p.fecha_entrega, p.estado, p.comentarios FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE (p.fecha_entrega > p.fecha_esperada) OR ((p.fecha_entrega IS NULL) AND (p.estado = 'Entregado'));
    
    /* Opcion propuesta por el profesor  */
    SELECT DISTINCT c.nombre_cliente, p.fecha_esperada, p.fecha_entrega, p.estado, p.comentarios FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE p.estado = 'Pendiente';
```

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

```sql
    SELECT DISTINCT c.nombre_cliente , g.gama FROM gama_producto g INNER JOIN producto p ON g.gama = p.gama INNER JOIN detalle_pedido d ON p.codigo_producto = d.codigo_producto INNER JOIN pedido pd ON d.codigo_pedido = pd.codigo_pedido INNER JOIN cliente c ON pd.codigo_cliente = c.codigo_cliente;
```
## Segundo listado de consultas 
### 1.4.6 Consultas multitabla (Composición externa)

**Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL LEFT JOIN y NATURAL RIGHT JOIN.**


1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
 
 ```sql
SELECT c.codigo_cliente AS CODIGO, c.nombre_cliente AS CLIENTE FROM cliente c LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente WHERE p.codigo_cliente IS NULL;

 ```
 
2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pedido.
 
 ```sql
SELECT c.nombre_cliente AS Clientes_no_pedido FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE p.codigo_pedido IS NULL;  

 ```
 
3. Devuelve un listado que muestre los clientes que no han realizado ningún pago y los que no han realizado ningún pedido.
 
 ```sql
SELECT c.codigo_cliente AS CODIGO_cliente, c.nombre_cliente AS CLIENTE  from cliente c LEFT JOIN pago p ON  c.codigo_cliente = p.codigo_cliente LEFT JOIN pedido pd ON c.codigo_cliente = pd.codigo_cliente  WHERE p.codigo_cliente IS NULL AND pd.codigo_pedido IS NULL; 

 ```
 
4. Devuelve un listado que muestre solamente los empleados que no tienen una oficina asociada.
 
 ```sql
SELECT e.codigo_empleado AS CODIGO_EMPLEADO, e.nombre AS EMPLEADO FROM empleado e LEFT JOIN oficina o ON e.codigo_oficina= o.codigo_oficina  WHERE o.codigo_oficina IS NULL;

 ```
 
5. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado.
 
 ```sql
SELECT e.nombre AS EMPLEADO FROM empleado e LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas WHERE c.codigo_cliente IS NULL;

 ```
 
6. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado junto con los datos de la oficina donde trabajan.
 
 ```sql
 SELECT e.codigo_empleado, e.nombre AS EMPLEADO, e.codigo_oficina, o.ciudad, o.pais FROM empleado e LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina WHERE c.codigo_cliente IS NULL;

 ```
 
7. Devuelve un listado que muestre los empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.
 
 ```sql
 SELECT e.codigo_empleado, e.nombre AS EMPLEADO FROM empleado e LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas WHERE o.codigo_oficina IS NULL OR c.codigo_cliente IS NULL;

 ```
 
8. Devuelve un listado de los productos que nunca han aparecido en un pedido.
 
 ```sql
 SELECT p.codigo_producto, p.nombre AS producto FROM producto p LEFT JOIN detalle_pedido dp ON p.codigo_producto = dp.codigo_producto WHERE dp.codigo_producto IS NULL;

 ```
 
9. Devuelve un listado de los productos que nunca han aparecido en un pedido. El resultado debe mostrar el nombre, la descripción y la imagen del producto.
 
 ```sql
SELECT DISTINCT p.nombre AS Nombre_Producto, p.descripcion AS Descripcion FROM producto p LEFT JOIN detalle_pedido dp ON p.codigo_producto = dp.codigo_producto WHERE dp.codigo_producto IS NULL;

 ```
 
10. Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.
 
 ```sql
SELECT o.codigo_oficina, o.ciudad, o.pais FROM oficina o WHERE o.codigo_oficina NOT IN (SELECT DISTINCT e.codigo_oficina FROM empleado e WHERE e.codigo_empleado IN (SELECT DISTINCT c.codigo_empleado_rep_ventas FROM cliente c WHERE c.codigo_cliente IN (SELECT DISTINCT p.codigo_cliente FROM pedido p INNER JOIN detalle_pedido dp ON p.codigo_pedido = dp.codigo_pedido INNER JOIN producto prod ON dp.codigo_producto = prod.codigo_producto INNER JOIN gama_producto gp ON prod.gama = gp.gama WHERE gp.gama = 'Frutales')));

 ```
 
11. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.
 
 ```sql
 SELECT c.codigo_cliente, c.nombre_cliente FROM cliente c INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente LEFT JOIN pago pa ON c.codigo_cliente = pa.codigo_cliente WHERE pa.codigo_cliente IS NULL;

 ```
 
12. Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado.
 
 ```sql
 SELECT e.codigo_empleado, e.nombre AS EMPLEADO, e.codigo_jefe, j.nombre AS JEFE FROM empleado e LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas WHERE c.codigo_cliente IS NULL;

 ```


## Tercer Listado de consultas 

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.
 ```sql
 SELECT codigo_oficina, ciudad FROM oficina 
 ```
2. Devuelve un listado con la ciudad y el telefono de las oficinas de España.
```sql
SELECT ciudad, telefono FROM oficina WHERE pais = 'España';

 ```
3.  Devuelve un listado con el nombre, apellidos y email de los empleados cuyo jefe tiene un código de jefe igual a 7 
```sql
SELECT nombre, CONCAT(apellido1,"  ",apellido2), email FROM empleado WHERE codigo_jefe = 7;

 ```
4. Devuelve el nombre del puesto, nombre apellidos y email del jefe de la empresa.
```sql
SELECT e.puesto, CONCAT(e.nombre,'',e.apellido1,e.apellido2) AS Nombre_completo, e.email FROM empleado e WHERE LOWER(e.puesto)='Director general';


 ```
5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.
```sql
SELECT e.nombre,CONCAT(e.apellido1,"   ", e.apellido2) AS Apellidos, e.puesto FROM empleado e WHERE LOWER(e.puesto)<>'Representante ventas';
 ``` 
6. Devuelve un listado con el nombre de todos los clientes españoles.
 ```sql
SELECT CONCAT(c.nombre_contacto,"  ",c.apellido_contacto) AS Nombre_clientes_españoles  FROM cliente c WHERE pais = 'spain';
  ``` 
7. Devuelve un listado con los distintos estados por los que puede pasar un pedido.
```sql

SELECT p.estado FROM pedido p;

 ```
8. Devuelve un listado con el código de cliente de aquellos clientes que realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:

* Utilizando la función Year de MySQL.
```sql
SELECT DISTINCT codigo_cliente FROM pago WHERE YEAR(fecha_pago) = 2008;

 ```
* Utilizando la función DATE FORMAT de MySQL.

```sql
SELECT DISTINCT codigo_cliente FROM pago WHERE DATE_FORMAT(fecha_pago, '%Y') = '2008';

 ```
* Sin utilizar ninguna de las funciones anteriores.
```sql
SELECT DISTINCT p.codigo_cliente FROM pago p JOIN pedido pe ON p.codigo_cliente = pe.codigo_cliente WHERE pe.fecha_pedido BETWEEN '2008-01-01' AND '2008-12-31';

 ```
9. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.
```sql
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega FROM pedido WHERE fecha_entrega > fecha_esperada;

 ```
10. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al menos dos días antes de la fecha esperada.

* Utilizando la función APoDATE de MySQL.
```sql
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega FROM pedido WHERE ADDDATE(fecha_entrega, 2) <= fecha_esperada;

 ```
* Utilizando la función DATEDIFF de MySQL.
```sql
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega FROM pedido WHERE DATEDIFF(fecha_esperada, fecha_entrega) >= 2;

 ```
* ¿Sería posible resolver esta consulta utilizando el operador de suma + oresta -?
```sql
No es posible 
 ```
11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009 .
```sql
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega FROM pedido WHERE estado = 'rechazado' AND YEAR(fecha_esperada) = 2009;

 ```
12. Devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.
```SQL
SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega FROM pedido WHERE MONTH(fecha_entrega) = 1;

```

13. Devuelve un listado con todos los pagos que se realizaron en el año 2008 mediante Paypal. Ordene el resultado de mayor a menor.
```SQL
SELECT * FROM pago WHERE YEAR(fecha_pago) = 2008 AND forma_pago = 'Paypal' ORDER BY total DESC;

```

14. Devuelve un listado con todas las formas de pago que aparecen en la tabla pago. Tenga en cuenta que no deben aparecer formas de pago repetidas.
```SQL
SELECT DISTINCT forma_pago FROM pago;

```

15. Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.
```SQL
SELECT * FROM producto WHERE gama = 'Ornamentales' AND cantidad_en_stock > 100 ORDER BY precio_venta DESC;

```

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y cuyo representante de ventas tenga el código de empleado 11 o 30.
```SQL
SELECT * FROM cliente WHERE ciudad = 'Madrid' AND codigo_empleado_rep_ventas IN (11, 30);

```

## CUARTO LISTADO DE CONSULTAS  

### 1.4.8 Subconsultas

#### 1.4.8.1 Con operadores básicos de comparación

1. Devuelve el nombre del cliente con mayor límite de crédito.
```sql
SELECT c.nombre_cliente, c.limite_credito FROM (SELECT nombre_cliente, limite_credito FROM cliente ORDER BY limite_credito DESC LIMIT 1) c;
```
2. Devuelve el nombre del producto que tenga el precio de venta más caro.
```sql
SELECT p.nombre, p.precio_venta FROM (SELECT nombre, precio_venta FROM producto ORDER BY precio_venta DESC LIMIT 1)p;
```
3. Devuelve el nombre del producto del que se han vendido más unidades. (Tenga en cuenta que tendrá que calcular cuál es el número total de unidades que se han vendido de cada producto a partir de los datos de la tabla `detalle_pedido`)
```sql
SELECT p.nombre AS Nombre_producto  FROM producto p WHERE p.codigo_producto = (SELECT dp.codigo_producto   FROM detalle_pedido dp GROUP BY dp.codigo_producto ORDER BY SUM(dp.cantidad) DESC LIMIT 1 ); 

```
4. Los clientes cuyo límite de crédito sea mayor que los pagos que haya realizado. (Sin utilizar `INNER JOIN`).
```sql
SELECT c.nombre_cliente, c.codigo_cliente, c.limite_credito FROM cliente c WHERE c.limite_credito > (SELECT IFNULL(SUM(total), 0) FROM pago WHERE c.codigo_cliente = pago.codigo_cliente);

```
5. Devuelve el producto que más unidades tiene en stock.
```sql
SELECT p.nombre  FROM producto p WHERE p.cantidad_en_stock=(SELECT MAX (cantidad_en_stock)FROM producto) ; 

```
6. Devuelve el producto que menos unidades tiene en stock.
```sql
SELECT p.nombre FROM producto p WHERE p.cantidad_en_stock=(SELECT MIN (cantidad_en_stock)FROM producto);

```
7. Devuelve el nombre, los apellidos y el email de los empleados que están a cargo de **Alberto Soria**.
```sql
SELECT e.nombre, CONCAT(e.apellido1,"  ", e.apellido2), e.email FROM empleado e WHERE e.codigo_empleado IN (SELECT codigo_jefe FROM empleado WHERE nombre = 'Alberto' AND apellido1 = 'Soria') ;

```

#### 1.4.8.2 Subconsultas con ALL y ANY

1. Devuelve el nombre del cliente con mayor límite de crédito.
```sql
SELECT  c.nombre_cliente, c.limite_credito
FROM cliente c
WHERE c.limite_credito >= ALL (SELECT MAX(limite_credito) FROM cliente); 
```
2. Devuelve el nombre del producto que tenga el precio de venta más caro.
```sql
SELECT p.nombre, p.precio_venta
FROM producto p 
WHERE p.precio_venta >= ALL (SELECT MAX(precio_venta) FROM producto); 
```
3. Devuelve el producto que menos unidades tiene en stock.
```sql
SELECT  p.nombre, cantidad_en_stock
FROM producto p
WHERE  p.cantidad_en_stock <= ALL (SELECT MIN(cantidad_en_stock) FROM producto); 
```

#### 1.4.8.3 Subconsultas con IN y NOT IN

1. Devuelve el nombre, apellido1 y cargo de los empleados que no representen a ningún cliente.
```sql
SELECT e.nombre, e.apellido1 AS  Primer_apellido, e.puesto 
FROM empleado e
WHERE codigo_empleado NOT IN (SELECT codigo_empleado_rep_ventas FROM cliente);  
```
2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
```sql
SELECT * FROM cliente
WHERE codigo_cliente NOT IN (SELECT codigo_cliente FROM pago); 
```
3. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.
```sql
SELECT *
FROM cliente
WHERE codigo_cliente IN (
    SELECT DISTINCT codigo_cliente
    FROM pago
);
```
4. Devuelve un listado de los productos que nunca han aparecido en un pedido.
```sql
SELECT *
FROM producto
WHERE codigo_producto NOT IN (
    SELECT DISTINCT codigo_producto
    FROM detalle_pedido
);

```
5. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no 
sean representante de ventas de ningún cliente.
```sql
SELECT e.nombre, e.apellido1, e.apellido2, e.puesto, o.telefono
FROM empleado e
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
WHERE e.codigo_empleado NOT IN (
    SELECT DISTINCT codigo_empleado_rep_ventas
    FROM cliente
    WHERE codigo_empleado_rep_ventas IS NOT NULL
);
```
6. Devuelve las oficinas donde **no trabajan** ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama `Frutales`.
```sql
SELECT *
FROM oficina o
WHERE o.codigo_oficina NOT IN (
    SELECT DISTINCT e.codigo_oficina
    FROM empleado e
    WHERE e.codigo_empleado NOT IN (
        SELECT DISTINCT codigo_empleado
        FROM cliente
        WHERE codigo_cliente IN (
            SELECT DISTINCT codigo_cliente
            FROM pedido pd
            JOIN detalle_pedido dp ON pd.codigo_pedido = dp.codigo_pedido
            JOIN producto pr ON dp.codigo_producto = pr.codigo_producto
            WHERE pr.gama = 'Frutales'
        )
    )
);

```
7. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.
```sql
SELECT *
FROM cliente 
WHERE codigo_cliente IN (
    SELECT DISTINCT codigo_cliente
    FROM pedido)
AND codigo_cliente NOT IN (
    SELECT DISTINCT codigo_cliente
    FROM pago 
);

```

#### 1.4.8.4 Subconsultas con EXISTS y NOT EXISTS

1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.
```sql
SELECT *
FROM cliente c
WHERE NOT EXISTS (
    SELECT 1
    FROM pago p
    WHERE p.codigo_cliente = c.codigo_cliente
);
```
2. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.
```sql
SELECT *
FROM cliente c
WHERE EXISTS (
    SELECT 1
    FROM pago p
    WHERE p.codigo_cliente = c.codigo_cliente
);
```
3. Devuelve un listado de los productos que nunca han aparecido en un pedido.
```sql
SELECT *
FROM producto pr
WHERE NOT EXISTS (
    SELECT 1
    FROM detalle_pedido dp
    WHERE dp.codigo_producto = pr.codigo_producto
);
```
4. Devuelve un listado de los productos que han aparecido en un pedido alguna vez.
```sql
SELECT *
FROM producto pr
WHERE EXISTS (
    SELECT 1
    FROM detalle_pedido dp
    WHERE dp.codigo_producto = pr.codigo_producto
);
```


### Consultas 5 TIPS SQL (Group By, Where, Update, Select)

#### Group By 
1. Multiple agrupamiento 
```sql

SELECT c.nombre_cliente, COUNT(p.codigo_pedido) AS total_pedidos
FROM cliente c
LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
GROUP BY c.nombre_cliente;

```
2. Muestra el nombre del producto y la cantidad total de productos por gama.
```sql
SELECT SUBSTRING(p.gama, 1, 1) AS primeraLetraGama, COUNT(*) AS totalProductos
FROM producto p
INNER JOIN gama_producto g ON p.gama = g.gama
GROUP BY SUBSTRING(p.gama, 1, 1);
```
3. Busca contar el número total de productos en cada gama, mostrando la primera letra de cada gama y la cantidad total de productos en esa gama.
```sql
SELECT SUBSTRING(p.gama, 1, 1) AS gamaLetter, COUNT(*) AS totalProductos
FROM producto p
INNER JOIN gama_producto g ON p.gama = g.gama
GROUP BY SUBSTRING(p.gama, 1, 1);
```
4. busca la cantidad mínima de crédito límite entre los clientes cuyos nombres de contacto comienzan con la letra 'A'. Además, muestra solo aquellos clientes cuyos nombres de contacto comienzan con 'A' y tienen más de 2 pedidos realizados.
```sql
SELECT c.nombre_cliente, MIN(c.limite_credito) AS MinLimiteCredito
FROM cliente c
INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
WHERE SUBSTRING(c.nombre_contacto, 1, 1) = 'A'
GROUP BY c.nombre_cliente
HAVING COUNT(*) > 2; 
```
5. cuenta el número total de empleados en cada oficina, mostrando el código de la oficina, el país al que pertenece y la cantidad total de empleados en esa oficina.
```sql
SELECT o.codigo_oficina, o.pais, COUNT(*) AS TotalEmpleados 
FROM empleado e 
INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
GROUP BY o.codigo_oficina, o.pais; 
```
#### WHERE 

1. NOt IN, enumere los productos cuya identificación no sea 'AR-001' ni 'FR-1'.

    ```sql
    SELECT codigo_producto 
    FROM producto 
    WHERE codigo_producto NOT IN('AR-001', 'FR-1');
    ```

2. Subconsulta, Lista los rangos que hay en el producto y que son mayores que cero.

    ```sql
    SELECT * 
    FROM gama_producto g 
    WHERE (SELECT count(*) FROM producto  WHERE producto.gama = g.gama) > 0;
    ```

3. REGEX, Nombre del producto donde contiene una 'y' o una 'z'.

    ```sql
    SELECT * 
    FROM producto 
    WHERE nombre REGEXP "[y-z]";
    ```

4. IN y Subconsulta, nombre del producto donde su rango comienza con A.

    ```sql
    SELECT * 
    FROM producto 
    WHERE gama IN(SELECT gama FROM gama_producto WHERE gama REGEXP '^A');
    ```


5. Funciones, Nombre del Producto donde su gama comienza con F.

    ```sql
    SELECT * 
    FROM producto 
    WHERE SUBSTRING(gama,1,1) = 'F';
    ```

#### UPDATE 
1. 
```sql
CREATE VIEW vista_json_data 
AS SELECT
	CAST(imagen AS JSON) as imagen_json
FROM gama_producto;
```
2. 
```sql
UPDATE producto
SET gama = 'No gama'
FROM producto
RIGHT JOIN producto.gama = gama.gama;
```
3.
```sql
UPDATE empleado
SET pueto = (
	SELECT puesto FROM empleado WHERE codigo_empleado = 1
)
WHERE codigo_empleado = 2
```
4.
```sql
UPDATE clientes
SET pais = DEFAULT
```
5.
```sql
UPDATE clientes
SET pais = pais + 'no';
```

#### SELECT 

# TIPS SELECT

1. Valores fijos, agregue una columna y asígnele un valor si no está en la tabla original.
    ```sql
    SELECT codigo_producto, nombre, gama, 1 AS ghost 
    FROM producto;
    ```

2. Operaciones con columnas, Conoce el total de dos columnas que están relacionadas, pero son tablas diferentes.

    ```sql
    SELECT DISTINCT d.precio_unidad, producto.precio_venta, (d.precio_unidad + producto.precio_venta) AS Suma FROM producto 
    INNER JOIN detalle_pedido d ON d.codigo_producto = producto.codigo_producto;
    ```


3. Condiciones, Listar productos y listar en 'Retornar' cuando son iguales a 'Membrillero' aparece un 1, pero si es 'Higuera' un 2 y si no está en la condición anterior devuelve un 3.

    ```sql
    SELECT nombre, CASE WHEN nombre = 'Membrillero' THEN 1 WHEN nombre = 'Higuera' THEN 2 ELSE 3 END AS Retornar 
    FROM producto;
    ```

4. Subconsultas, Lista los rangos y el total de rangos que hay para cada producto.

    ```sql
    SELECT gama, (SELECT COUNT(producto.gama) FROM producto WHERE producto.gama = gama_producto.gama) AS Total_gamas_productos 
    FROM gama_producto;
    ```

5. Consulta en subconsulta, enumera los rangos y el número total de rangos para cada producto, pero mayor que 4.

    ```sql
    SELECT gama, Total_gama 
    FROM (SELECT g.gama, COUNT(g.gama) AS Total_gama FROM producto INNER JOIN gama_producto g ON g.gama = producto.gama GROUP BY g.gama) AS MiTabla 
    WHERE MiTabla.Total_gama > 4;
    ```

6. Extra: Columna virtual autoincremental, enumere una identificación de autoincremental, el código de producto en relación con el autoincremental y enumere el nombre del producto.

    ```sql
    SELECT ROW_NUMBER() OVER (ORDER BY (SELECT 1)) AS MiId, codigo_producto, nombre
    FROM producto ORDER BY nombre;
    ```
