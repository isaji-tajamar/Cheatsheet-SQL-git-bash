# Cheatsheet SQL · Git · Bash

Este repositorio contiene una chuleta práctica en Markdown con los comandos y consultas más útiles de:

- SQL: consultas básicas, filtros, inserciones, actualizaciones, borrados, agrupaciones, uniones y modificaciones de tablas.
- Git: flujo de trabajo básico, ramas, remotos, deshacer cambios, stash y tags.
- Bash: navegación de archivos, permisos, búsqueda, procesos, compresión y ejemplos de scripting.

Cada comando o consulta incluye un comentario inline para recordar rápidamente su propósito.

---

## SQL - Consultas básicas

### SELECT
```sql
SELECT columna1, columna2        -- selecciona columnas
FROM tabla                       -- tabla origen
WHERE condicion                  -- condición de filtrado
ORDER BY columna1 ASC            -- ordenar resultados
LIMIT 10 OFFSET 0;               -- limitar y paginar
```

### WHERE
```sql
SELECT *
FROM tabla
WHERE columna = 'valor' AND otra_columna > 10;   -- filtra filas según condiciones
```

### INSERT
```sql
INSERT INTO tabla (col1, col2)
VALUES ('valor1', 'valor2');    -- inserta nuevas filas
```

### UPDATE
```sql
UPDATE tabla
SET columna = 'nuevo_valor'
WHERE id = 1;                   -- actualiza filas existentes
```

### DELETE
```sql
DELETE FROM tabla
WHERE id = 1;                   -- elimina filas específicas
```

### ORDER BY
```sql
SELECT *
FROM tabla
ORDER BY columna1 ASC, columna2 DESC;  -- ordena resultados
```

### GROUP BY
```sql
SELECT categoria, COUNT(*) AS total
FROM productos
GROUP BY categoria
HAVING COUNT(*) > 5;            -- agrupa resultados y aplica funciones
```

### JOIN
```sql
SELECT a.id, b.nombre
FROM tablaA a
INNER JOIN tablaB b ON a.id_b = b.id;
-- INNER JOIN: solo coincidencias
-- LEFT JOIN: todas las filas de A y coincidencias de B
-- RIGHT JOIN: todas las filas de B y coincidencias de A
```

### CREATE
```sql
CREATE TABLE empleados (
  id SERIAL PRIMARY KEY,
  nombre VARCHAR(100),
  salario DECIMAL(10,2)
);                               -- crea una tabla nueva
```

### ALTER
```sql
ALTER TABLE empleados ADD COLUMN departamento VARCHAR(50);
ALTER TABLE empleados DROP COLUMN salario;
ALTER TABLE empleados RENAME COLUMN nombre TO nombre_completo;
-- modifica estructura de tablas
```

---

## Git - Control de versiones

### Flujo básico
```bash
git init                       # inicializa repositorio local
git clone URL                  # clona repositorio remoto
git status                     # muestra estado
git add .                      # añade cambios
git commit -m "mensaje"        # crea commit
git log --oneline --graph      # historial resumido
```

### Ramas
```bash
git branch                     # listar ramas
git switch -c nueva-rama       # crear y cambiar a nueva rama
git merge otra-rama            # fusionar rama en la actual
git rebase main                # reescribir historial sobre main
```

### Remotos
```bash
git remote -v                  # ver remotos
git push origin main           # subir cambios
git pull                       # traer y fusionar
git fetch                      # traer sin fusionar
```

### Deshacer
```bash
git restore archivo.ext        # descartar cambios locales
git reset --soft HEAD~1        # deshacer commit, mantener cambios
git reset --hard HEAD~1        # deshacer commit y cambios (⚠️ peligroso)
git revert <commit>            # revertir commit sin romper historial
```

### Stash
```bash
git stash push -m "wip"        # guardar cambios temporales
git stash list                 # listar stashes
git stash apply stash@{0}      # aplicar stash
git stash drop stash@{0}       # eliminar stash
```

### Tags
```bash
git tag v1.0.0                 # crear tag de versión
git push origin v1.0.0         # subir tag
```

---

## Bash - Comandos esenciales

### Navegación
```bash
pwd                            # ruta actual
ls -la                         # listar en detalle
cd /ruta/                      # cambiar de directorio
mkdir carpeta                  # crear directorio
touch archivo.txt              # crear archivo vacío
```

### Archivos
```bash
cp origen destino              # copiar
mv origen destino              # mover/renombrar
rm archivo                     # borrar archivo
rm -r carpeta                  # borrar carpeta y contenido
```

### Contenido
```bash
cat archivo.txt                # mostrar contenido
head archivo.txt               # primeras líneas
tail archivo.txt               # últimas líneas
grep "texto" archivo.txt       # buscar texto
```

### Procesos
```bash
ps aux                         # ver procesos
top                            # procesos en tiempo real
kill <PID>                     # terminar proceso
jobs                           # ver trabajos en background
fg %1                          # traer job al foreground
```

### Permisos
```bash
chmod 755 script.sh            # cambiar permisos
chown usuario:grupo archivo    # cambiar propietario
```

### Red y descargas
```bash
ip a                           # interfaces de red
ss -tulpen                     # puertos abiertos
curl -I https://ejemplo.com    # cabeceras HTTP
scp archivo user@host:/ruta/   # copiar archivo vía SSH
ssh user@host                  # conectar vía SSH
```

### Disco
```bash
df -h                          # uso de disco
du -sh *                       # tamaño de carpetas/archivos
```

### Scripts
```bash
# variable
NOMBRE="Iñigo"                 # asignar valor
echo "Hola $NOMBRE"            # usar variable

# bucle
for f in *.txt; do             # recorrer todos los .txt
  echo "$f"
done

# condicional
if [ -f "config.yml" ]; then   # si existe archivo
  echo "Config existe"
fi
```
