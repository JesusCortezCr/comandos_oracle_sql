# 🗄 CURSO ORACLE – COMANDOS  
## 📦 Gestión de Almacenamiento

La gestión de almacenamiento en Oracle permite administrar el espacio donde se guardarán:

- Tablas  
- Índices  
- Vistas  
- Datos  

---

## 📊 Estructura de almacenamiento en Oracle

```
Base de Datos (ORCL)
        ↓
   Tablespaces
        ↓
    Datafiles (.dbf)
        ↓
   Datos reales en disco
```

---

## 🏗 Analogía simple

Imagina un edificio:

- 🏢 **Base de datos** = Edificio  
- 🏬 **Tablespace** = Un piso  
- 🧱 **Datafile** = El concreto del piso  
- 🗄 **Tabla** = Una oficina dentro del piso  

---

# 📁 Tablespace

Un **Tablespace** es un contenedor lógico de almacenamiento dentro de la base de datos.

- No es un archivo físico.
- Organiza dónde se guardan las tablas.
- Permite administrar el espacio.

---

# 💾 Datafile

Un **Datafile** es un archivo físico donde realmente se guardan los datos.

- Extensión: `.dbf`
- Vive en el sistema operativo.
- Contiene los bloques de datos reales.

---

# 🛠 COMANDOS PRINCIPALES

---

## 1️⃣ Crear directorio en Linux

```bash
mkdir /u02/datoscortez
```

Verificar:

```bash
ls /u02
```

---

## 2️⃣ Crear Tablespace

### Tablespace simple (1 datafile)

```sql
CREATE TABLESPACE TBS_CORTEZ 
DATAFILE '/u02/datoscortez/df_cortez01.dbf' 
SIZE 50M;
```

---

### Tablespace con múltiples datafiles

```sql
CREATE TABLESPACE TBS_SEGURIDAD 
DATAFILE 
'/u02/datos/df_seg_01.dbf' SIZE 10M,
'/u02/datos/df_seg_02.dbf' SIZE 10M;
```

---

## 3️⃣ Consultar Tablespaces

```sql
SELECT * FROM DBA_TABLESPACES;
```

---

## 4️⃣ Consultar Datafiles

```sql
SELECT * FROM DBA_DATA_FILES;
```

---

## 5️⃣ Consulta detallada de tamaños de Datafiles

```sql
SELECT 
    df.file_name AS datafile,
    df.tablespace_name AS tablespace,
    ts.bigfile AS is_bigfile,
    df.bytes / 1024 / 1024 AS Tamano_Mb,
    df.autoextensible,
    df.maxbytes / 1024 / 1024 / 1024 AS Tamano_maximo_Gb,
    ts.block_size
FROM 
    dba_data_files df
JOIN 
    dba_tablespaces ts 
ON df.tablespace_name = ts.tablespace_name
ORDER BY 
    df.tablespace_name, df.file_name;
```

---

## 6️⃣ Crear tabla dentro de un Tablespace

```sql
CREATE TABLE ARTICULO 
(
    ID INTEGER, 
    DESCRIPCION CHAR(100)
) 
TABLESPACE TBS_VENTAS;
```

---

## 7️⃣ Insertar registros masivos

```sql
INSERT INTO ARTICULO
SELECT LEVEL, 'ART_' || LEVEL
FROM DUAL
CONNECT BY LEVEL <= 10000;
```

---

## 8️⃣ Modificar tamaño de un Datafile

```sql
ALTER DATABASE 
DATAFILE '/u02/datos/df_ventas_01.dbf'
RESIZE 10M;
```

---

## 9️⃣ Agregar un nuevo Datafile a un Tablespace

```sql
ALTER TABLESPACE TBS_VENTAS
ADD DATAFILE '/u02/datos/df_ventas_02.dbf' 
SIZE 10M;
```

---

## 🔟 Eliminar un Tablespace (con todo su contenido)

⚠ Esto elimina tablas y archivos físicos.

```sql
DROP TABLESPACE TBS_VENTAS
INCLUDING CONTENTS AND DATAFILES;
```

---

# 🧠 Conceptos Clave

- Los datafiles están divididos en **bloques de datos**.
- El tamaño por defecto suele ser **8KB**.
- Pueden existir bloques de: 2K, 4K, 8K, 16K, 32K.
- Para datos grandes (como BLOB, PDFs), se recomienda bloques mayores (16K o 32K).

---

# 📌 Resumen Final

- La base de datos contiene tablespaces.
- Los tablespaces contienen datafiles.
- Los datafiles contienen los datos reales.
- Las tablas pueden asignarse a un tablespace específico.
- Se puede:
  - Redimensionar datafiles
  - Agregar nuevos datafiles
  - Consultar uso y tamaños
  - Eliminar tablespaces
# 🗄 CURSO ORACLE – CREACIÓN Y GESTIÓN DE TABLAS

## 📌 Tabla con Primary Key Autoincremental

En Oracle 12c en adelante se puede usar `GENERATED AS IDENTITY` para crear un campo autoincremental sin necesidad de sequence ni trigger.

---

## 🏗 Creación de Tabla

```sql
CREATE TABLE CLIENTES(
    ID NUMBER GENERATED ALWAYS AS IDENTITY,
    NOMBRE VARCHAR2(100) NOT NULL,
    EMAIL VARCHAR2(150),
    FECHA_REGISTRO DATE DEFAULT SYSDATE,
    CONSTRAINT PK_CLIENTES PRIMARY KEY (ID)
) TABLESPACE TBS_CORTEZ;
```

### 🔎 Explicación

- `GENERATED ALWAYS AS IDENTITY` → Autoincremental automático
- `PRIMARY KEY (ID)` → Clave primaria
- `NOT NULL` → Campo obligatorio
- `DEFAULT SYSDATE` → Inserta fecha actual automáticamente
- `TABLESPACE TBS_CORTEZ` → Guarda la tabla en ese tablespace

---

## ➕ Insertar Datos

```sql
INSERT INTO CLIENTES (NOMBRE,EMAIL) 
VALUES ('JESUS CORTEZ','JESUS@GMAIL.COM');

COMMIT;
```

> No se coloca el ID porque Oracle lo genera automáticamente.

---

## 👀 Ver Datos de la Tabla

```sql
SELECT * FROM CLIENTES;
```

---

## 🔎 Ver Estructura de la Tabla

```sql
DESCRIBE CLIENTES;
```

---

## 🗑 Eliminar Tabla

```sql
DROP TABLE CLIENTES;
```

⚠ Esto elimina:
- La tabla
- Todos sus datos
- Restricciones
- Índices

---

# 📌 Resumen

| Comando | Función |
|----------|----------|
| CREATE TABLE | Crear tabla |
| INSERT INTO | Insertar datos |
| COMMIT | Guardar cambios |
| SELECT | Consultar datos |
| DESCRIBE | Ver estructura |
| DROP TABLE | Eliminar tabla |

---

🚀 Ahora ya sabes:

- Crear tabla con PK autoincremental
- Insertar datos
- Consultar datos
- Ver estructura
- Eliminar tabla

