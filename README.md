# Cheatsheet SQL · Git · Bash

---

##  SQL - Consultas

```sql
SELECT columna1, columna2        -- selecciona columnas
FROM tabla                       -- tabla origen
WHERE condicion                  -- condición de filtrado
ORDER BY columna1 ASC            -- ordenar resultados
LIMIT 10 OFFSET 0;               -- limitar y paginar
```

```sql
WHERE col = 10                   -- igualdad exacta
AND otra_col BETWEEN 5 AND 15    -- rango (incluye extremos)
AND texto ILIKE '%abc%'          -- búsqueda parcial (ignora mayúsculas, PostgreSQL)
AND fecha >= CURRENT_DATE - INTERVAL '7 days'  -- últimos 7 días
AND col IS NOT NULL              -- excluir valores nulos
AND col IN (1,2,3);              -- lista de valores
```

```sql
-- INNER JOIN: solo coincidencias
SELECT a.*, b.*
FROM A a
JOIN B b ON b.a_id = a.id;

-- LEFT JOIN: todo A aunque no tenga coincidencia en B
SELECT a.*, b.*
FROM A a
LEFT JOIN B b ON b.a_id = a.id;
```

```sql
SELECT categoria, COUNT(*) AS n, AVG(precio) AS media
FROM productos
GROUP BY categoria                -- agrupar por categoría
HAVING COUNT(*) > 5               -- filtrar sobre agregados
ORDER BY n DESC;                  -- ordenar por número de productos
```

```sql
SELECT id,
  CASE
    WHEN total >= 100 THEN 'VIP'   -- condicional: si >=100 → VIP
    WHEN total >= 50  THEN 'MEDIO' -- si >=50 → MEDIO
    ELSE 'BÁSICO'                  -- resto → BÁSICO
  END AS nivel
FROM clientes;
```

```sql
-- Subconsulta en SELECT
SELECT id, (SELECT COUNT(*) FROM pedidos p WHERE p.cliente_id = c.id) AS pedidos
FROM clientes c;

-- CTE (WITH): subconsulta reutilizable
WITH ventas AS (
  SELECT cliente_id, SUM(importe) total FROM pedidos GROUP BY cliente_id
)
SELECT c.id, c.nombre, v.total
FROM clientes c
JOIN ventas v ON v.cliente_id = c.id;
```

```sql
SELECT
  id, cliente_id, importe,
  SUM(importe) OVER (PARTITION BY cliente_id ORDER BY fecha) AS acumulado,  -- acumulado
  ROW_NUMBER() OVER (PARTITION BY cliente_id ORDER BY fecha DESC) AS rn     -- ranking
FROM pedidos;
```

```sql
INSERT INTO tabla (a,b,c) VALUES (1,'x',NOW());   -- insertar fila
UPDATE tabla
SET campo = 'valor', actualizado_en = NOW()       -- actualizar fila
WHERE id = 123;
DELETE FROM tabla WHERE id = 123;                 -- borrar fila
```

```sql
BEGIN;                                            -- iniciar transacción
  UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1; -- quitar saldo
  UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2; -- añadir saldo
COMMIT;                                           -- confirmar (o ROLLBACK para deshacer)
```

---

## Git - Control de versiones

```bash
git init                       # inicializa repositorio local
git clone URL                  # clona repositorio remoto
git status                     # estado actual
git add .                      # añade todos los cambios
git commit -m "mensaje"        # guarda cambios en commit
git log --oneline --graph      # historial resumido y gráfico
```

```bash
git branch                     # lista ramas
git switch -c nueva-rama       # crea y cambia a nueva rama
git merge otra-rama            # fusiona rama en la actual
git rebase main                # reescribe base (historial más limpio)
```

```bash
git remote -v                  # ver remotos configurados
git push origin main           # subir cambios
git pull                       # traer y fusionar
git fetch                      # traer sin fusionar
```

```bash
git restore archivo.ext        # descarta cambios locales
git reset --soft HEAD~1        # deshace último commit (mantiene cambios staged)
git reset --hard HEAD~1        # borra commit y cambios
git revert <commit>            # crea commit inverso (seguro en remoto)
```

```bash
git stash push -m "wip"        # guardar cambios temporales
git stash list                 # listar stashes
git stash apply stash@{0}      # aplicar stash
git stash drop stash@{0}       # eliminar stash
```

```bash
git tag v1.0.0                 # crear tag de versión
git push origin v1.0.0         # subir tag
```

---

## Bash - Comandos esenciales

```bash
pwd                            # ruta actual
ls -la                         # listar en detalle
cd /ruta/                      # cambiar directorio
touch file.txt                 # crear archivo vacío
mkdir -p a/b/c                 # crear árbol de carpetas
cp origen dest                 # copiar archivo
mv origen dest                 # mover/renombrar
rm archivo                     # eliminar archivo
rm -r carpeta                  # eliminar carpeta y su contenido
```

```bash
chmod u+x script.sh            # dar permiso de ejecución
chmod 644 file                 # rw-r--r-- (usuario rw, grupo r, otros r)
chown usuario:grupo file       # cambiar propietario
```

```bash
grep -R "texto" .              # buscar texto en archivos
grep -Rni "texto" .            # incluir nº de línea, ignore case
find . -name "*.log"           # buscar archivos por nombre
find . -type f -mtime -1       # archivos modificados en <1 día
```

```bash
ps aux | grep nombre           # listar procesos por nombre
top                            # ver procesos en tiempo real
kill -TERM <pid>               # terminar proceso
kill -9 <pid>                  # forzar terminar
jobs                           # ver trabajos en segundo plano
bg %1                          # enviar trabajo al background
fg %1                          # traer trabajo al foreground
```

```bash
ip a                           # ver interfaces de red
ss -tulpen                     # ver puertos abiertos
curl -I https://ejemplo.com    # cabeceras HTTP
wget URL                       # descargar archivo
scp archivo user@host:/ruta/   # copiar archivo vía SSH
ssh user@host                  # conectar por SSH
```

```bash
df -h                          # uso de discos
du -sh *                       # tamaño de carpetas/archivos
```

```bash
# variable
NOMBRE="Iñigo"                 # asignar valor
echo "Hola $NOMBRE"            # mostrar variable

# bucle
for f in *.txt; do             # recorrer todos los .txt
  echo "$f"
done

# condicional
if [ -f "config.yml" ]; then   # si existe el archivo
  echo "Config existe"
fi
```

