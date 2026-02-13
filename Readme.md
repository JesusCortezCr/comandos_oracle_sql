<h1>CURSO ORACLE COMANDOS</h1>
<hr>
<h2>GESTION DE ALMACENAMIENTO</h2>
Gestion de area del almacenamiento donde se guardaran las tablas , indices , vistas , datos.
Base de datos (ORCL)
     ↓
Tablespaces
     ↓
Datafiles (.dbf)
     ↓
Datos reales en disco
🏗 Analogía simple
Imagina un edificio:

🏢 Base de datos = edificio

🏬 Tablespace = un piso

🧱 Datafile = el concreto del piso

🗄 Tabla = una oficina dentro del piso
<h3>TableSpace</h3>
Contenedor logico donde de almecenamiento dentro de una base de datos. No es un archivo fisico . Organiza donde se guarda las tablas y permite administrar el espacio.
<img width="752" height="285" alt="image" src="https://github.com/user-attachments/assets/ef23b634-be9f-498d-be7c-e33086d8b652" />COMANDOS O
<h3>Datafile</h3>
Archivo fisico donde realmente se guardan los datos. Tienen extension .dbf , viven en el sistema operativo y contienen los bloques de datos reales.
<h3>COMANDOS</h3>
--CREACION DIRECTORIO DONDE SE GUARDARAN TODOS LOS ARCHIVOS
mkdir /u02/datoscortez
--verificar directorio creado
ls /u02
--CREACION DE TABLESPACE
CREATE TABLESPACE TBS_CORTEZ DATAFILE '/u02/datoscortez/df_cortez01.dbf' SIZE 50M;
--CREACION DE TABLESPACE CON DATAFILES DENTRO
CREATE TABLESPACE TBS_SEGURIDAD 
DATAFILE 
'/u02/datos/df_seg_01.dbf' SIZE 10M , 
'/u02/datos/df_seg_02.dbf' SIZE 10M ;
--CONSULTAR LOS TABLESPACE
SELECT * FROM DBA_TABLESPACES;
--CONSULTA ADMINISTRATIVA DE DATAFILES;
SELECT * FROM DBA_DATA_FILES;
-- 5. CONSULTA QUE MUESTRA TAMAÑOS DE DATAFILES 
SELECT 
df.file_name datafile, 
df.tablespace_name tablespace, 
ts.bigfile AS is_bigfile, 
df.bytes / 1024 / 1024  AS Tamaño_Mb, 
df.autoextensible, 
df.maxbytes / 1024 / 1024 /1024 AS Tamaño_maximo_Gb, 
ts.block_size 
FROM 
dba_data_files df 
JOIN 
dba_tablespaces ts ON df.tablespace_name = ts.tablespace_name 
ORDER BY 
df.tablespace_name, df.file_name;

-- 6. CREANDO TABLA EN EL TABLESPACE   
CREATE TABLE ARTICULO 
(ID INTEGER, DESCRIPCION CHAR(100)) TABLESPACE TBS_VENTAS; 

--INSERCION DE REGISTROS 
INSERT INTO ARTICULO 
SELECT LEVEL, 'ART_' || LEVEL FROM DUAL 
CONNECT BY LEVEL <= 10000;

--7. MODIFICANDO TAMAÑO DE DATAFILES 
ALTER DATABASE 
DATAFILE '/u02/datos/df_ventas_01.dbf' 
RESIZE 10M; 

-- 8. AGREGANDO UN NUEVO DATAFILE AL TBS 
ALTER TABLESPACE TBS_VENTAS 
ADD DATAFILE '/u02/datos/df_ventas_02.dbf' SIZE 10M;
-- 9. ELIMINANDO LOS TBS 
DROP TABLESPACE TBS_VENTAS 
INCLUDING CONTENTS AND DATAFILES;

