# Apuntes de base de datos
```sql
SELECT p.nombre,GROUP_CONCAT(e.nombre SEPARATOR ',') FROM persona_etiqueta pe
INNER JOIN persona p ON pe.id_persona = p.id
INNER JOIN etiqueta e ON pe.id_etiqueta = e.id
WHERE p.id = 1 AND
e.id = pe.id_etiqueta;
```
