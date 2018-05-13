# Apuntes de base de datos

# Consulta de: Marcelo Gatica
```sql
SELECT p.nombre,GROUP_CONCAT(e.nombre SEPARATOR ',') FROM persona_etiqueta pe
INNER JOIN persona p ON pe.id_persona = p.id
INNER JOIN etiqueta e ON pe.id_etiqueta = e.id
WHERE p.id = 1 AND
e.id = pe.id_etiqueta;
```

# Procedimiento de Franco Barrera
```sql
CREATE TABLE test(-- DROP TABLE test;
    id INT AUTO_INCREMENT,
    i INT,
    texto1 VARCHAR(50),
    texto2 VARCHAR(50),
    PRIMARY KEY (id)    
);-- SELECT * FROM test


DELIMITER $$
CREATE PROCEDURE split (cadena TEXT, separador VARCHAR(20))-- DROP PROCEDURE split;
BEGIN
 
    DECLARE item_array TEXT;
    DECLARE i INT;
 
    SET i = 1; # se le puede dar cualquier valor menos 0.
 
    # INTRO BUCLE
 
    WHILE i > 0 DO
 
        SET i = INSTR(cadena,separador); 
        # seteo i a la posicion donde esta el caracter para separar
        # realiza lo mismo que indexOf en javascript
 
        SET item_array = SUBSTRING(cadena,1,i-1); 
        # esta variable guardara el valor actual del supuesto array
        # se logra cortando desde la posicion 1 que para MySQL es la primera letra (en javascript es 0)
        # hasta la posicion donde se encuentra la cadena a separar -1 ya que sino incluiria el 1er caracter
        # del caracter o cadena de caracteres que hacen de separador
        
        IF i > 0 THEN
        
            SET cadena = SUBSTRING(cadena,i+CHAR_LENGTH(separador),CHAR_LENGTH(cadena));
                
        # corto / preparo la cadena total para la proxima vez que se entre al bucle para eso corto desde la posicion
        # donde esta el caracter separador hasta el tamaño total de la cadena
        # como el separador puede ser de n caracteres en el 2do parametro paso i que es la posicion del separador
        # sumado al tamaño de su cadena 
 
        ELSE
        
        # si el if entra aca es porque i ya vale 0 y no entrara nuevamente al bucle lo cual significa que la 
        # cadena original ya no tiene separadores por ende lo que queda de ella es igual a la ultima posicion
        # del supuesto array
 
            SET item_array = cadena;
 
        
        END IF;
        
        # he creado una tabla test que tiene como estructura:
        # id int, i int, texto1 text, texto2 text para subir de muestra como cambia el indice (i)
        # y como sube el elemento iterado y por ultimo la cadena original para ver como va mutando
 
        INSERT INTO test (i,texto1,texto2) VALUES (i,item_array,cadena);
 
    END WHILE;
 
END $$
DELIMITER ;

CALL split('hola,como,estas',',');
```
