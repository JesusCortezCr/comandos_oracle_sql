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
