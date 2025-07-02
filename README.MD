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

### Notas 
-- Triggers
-- Marcar una cuota como pagada cuando se hace un pago
DELIMITER $$
CREATE TRIGGER actualizar_estado_cuota
AFTER INSERT ON pagos
FOR EACH ROW
BEGIN
  UPDATE cuotas_manejo
  SET estado_pago = 'Pagada'
  WHERE id_tarjeta = NEW.id_tarjeta;
END$$
DELIMITER ;


### Eventos

-- Crear evento que cada día revise cuotas vencidas
CREATE EVENT IF NOT EXISTS marcar_cuotas_vencidas
ON SCHEDULE EVERY 1 DAY
DO
  UPDATE cuotas_manejo
  SET estado_pago = 'Vencida'
  WHERE fecha_cobro < CURDATE() AND estado_pago = 'Pendiente';
-- Activar eventos :

SET GLOBAL event_scheduler = ON;
## Roles 

CREATE ROLE analista;
GRANT SELECT, EXECUTE ON banco_cl.* TO analista;

CREATE USER 'usuario1'@'localhost' IDENTIFIED BY '1234';
GRANT analista TO 'usuario1'@'localhost';


## Basicas sql

SELECT tipo_tarjeta, COUNT(*) AS total
FROM tarjetas_credito
GROUP BY tipo_tarjeta;

## Funciones 
DELIMITER $$
CREATE FUNCTION calcular_edad(fecha_nac DATE)
RETURNS INT
DETERMINISTIC
BEGIN
  RETURN TIMESTAMPDIFF(YEAR, fecha_nac, CURDATE());
END$$
DELIMITER ;

## Procedimientos almacenados

DELIMITER $$
CREATE PROCEDURE registrar_pago(
  IN p_id_tarjeta INT,
  IN p_monto DECIMAL(10,2)
)
BEGIN
  INSERT INTO pagos(id_tarjeta, monto_pagado)
  VALUES (p_id_tarjeta, p_monto);
END$$
DELIMITER ;
## Eventos programador 
CREATE EVENT actualizar_cuotas
ON SCHEDULE EVERY 1 DAY
DO
  UPDATE cuotas_manejo SET estado_pago = 'Vencida'
  WHERE fecha_cobro < CURDATE();
## Group by :

SELECT tipo_tarjeta, COUNT(*) AS total
FROM tarjetas_credito
GROUP BY tipo_tarjeta;
