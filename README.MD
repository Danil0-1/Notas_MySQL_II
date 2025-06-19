# Notas MYSQL_II

1. **Procedimientos almacenados** (`PROCEDURE`)

Son **bloques de código que guardas en la base de datos** y puedes ejecutar cuando los necesites. Son como **funciones en programación**, pero no devuelven valor directamente.

👉 Se usan para tareas repetitivas, como **agregar un pedido completo**, **actualizar precios**, **cancelar cosas**, etc.

`CREATE PROCEDURE saludar()
BEGIN
           SELECT 'Hola, mundo!';
END;`

Y se ejecuta asi :

`CALL saludar();`

2. **Funciones almacenadas** (`FUNCTION`)

Son como procedimientos, **pero devuelven un valor**. Se usan para **calcular** algo.

`CREATE FUNCTION sumar(a INT, b INT) RETURNS INT
BEGIN
     RETURN a + b;
END;`<>

`SELECT sumar(3, 5); -- Resultado: 8`

### ⚙️ 3. **Triggers** (Disparadores)

**acciones automáticas** que se ejecutan **cuando pasa algo en una tabla** (por ejemplo, cuando insertas, actualizas o borras un dato).

`CREATE TRIGGER antes_de_insertar_cliente
BEFORE INSERT ON cliente
FOR EACH ROW
BEGIN
SET NEW.nombre = UPPER(NEW.nombre); -- convierte a mayúsculas
END;`