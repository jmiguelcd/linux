# Automatización de Respaldos con Cron 🚀

Aprenderemos a crear un respaldo compatible con Windows y a programar el linux para que lo ejecute automáticamente.

## 1. Instalación de herramientas
No todos los sistemas Linux vienen con la herramienta de compresión ZIP instalada por defecto. Vamos a instalarla para asegurar que nuestro script funcione.

Copia y pega estos comandos:
```bash
# Actualizamos la lista de paquetes
sudo apt update

# Instalamos el compresor zip
sudo apt install zip -y
```

## 2. Creación del Script de Respaldo Profesional
Vamos a crear un nuevo script que use el formato `.zip`. Este archivo será guardado en una carpeta temporal y se nombrará automáticamente con la fecha y hora.

### Paso 1: Crea carpeta que vamos a respaldar, y crear el archivo con el script
```bash
sudo mkdir /ejercicio/backup
sudo echo "Archivo de prueba 1" > /ejercicio/backup/archivo1.txt
sudo echo "Archivo de prueba 2" > /ejercicio/backup/archivo2.txt
sudo mkdir /ejercicio/almacenamiento
sudo nano /ejercicio/respaldo.sh
```

### Paso 2: Pega el siguiente código
```bash
#!/bin/bash

# --- 1. CONFIGURACIÓN ---
# Carpeta que queremos proteger
ORIGEN="/ejercicio/backup"
# Carpeta donde se guardará el respaldo
DESTINO="/ejercicio/almacenamiento"
# Nombre del archivo con año, mes, día, hora y minuto
NOMBRE="$(date +%Y%m%d_%H%M).zip"
# Día del mes actual
DIA_HOY=$(date +%d)

# --- 2. PREPARACIÓN ---
# Si la carpeta de destino no existe, la creamos
sudo mkdir -p $DESTINO

# --- 3. CREAR EL RESPALDO ---
# Usamos zip -r (recursivo) para incluir todo lo que hay dentro
zip -r $DESTINO/$NOMBRE $ORIGEN
echo "✅ Respaldo completado: $DESTINO/$NOMBRE"

# --- 4. LIMPIEZA AUTOMÁTICA (Día 5) ---
if [ "$DIA_HOY" == "05" ]; then
    echo "🧹 Hoy es día 5. Borrando archivos viejos para ahorrar espacio..."
    # Busca archivos .zip con más de 30 días y los elimina
    find $DESTINO -name "*.zip" -mtime +30 -exec rm {} \;
    echo "✨ Limpieza terminada."
else
    echo "ℹ️ Hoy no es día de limpieza. Solo se generó el nuevo respaldo."
fi
```

## 3. Permisos y Prueba Manual
Antes de automatizarlo, debemos darle permiso de ejecución y probar que funcione.

```bash
# Damos permisos
sudo chmod +x /ejercicio/respaldo.sh

# Lo corremos manualmente por primera vez
sudo sh /ejercicio/respaldo.sh
```

## 4. Programación con Cron
`Cron` es un servicio que ejecuta tareas a una hora específica. Para configurarlo, usamos el comando `crontab`.

### Entender los tiempos de Cron:
El formato son 5 espacios: `Minuto Hora Día Mes Día_Semana`
* `0 0 * * *` -> Significa: "Todos los días a las 00:00 (medianoche)".
* `30 23 * * *` -> Significa: "Todos los días a las 11:30 PM".

### Pasos para programar tu script:
1. Abre el configurador de tareas:
   ```bash
   sudo crontab -e
   ```
   *(Si te pregunta qué editor usar, elige el número que corresponda a **nano**).*

2. Ve hasta el puro final del archivo y pega esta línea:
   ```bash
   0 23 * * * /bin/sh /ejercicio/respaldo.sh
   ```
   *(Esto hará que tu respaldo se cree solito todas las noches a las 11:00 PM).*

3. Guarda y sal (`CTRL + X`, luego `y`, luego `ENTER`).

> **Nota de Seguridad:** Al usar `sudo crontab -e`, el script se ejecutará con permisos de administrador (root), lo cual es necesario para respaldar carpetas protegidas.

> **Nota de Ingeniería:** Al usar /bin/sh definimos explícitamente el intérprete, lo que garantiza que la tarea no dependa de las variables de entorno de la sesión de usuario.

## 5. Tarea: Transferencia y Verificación en Windows 🪟 (o MacOS)
Vamos a descargar FileZilla, de acuerdo a tu sistema operativo y version:

*(Nota: Presiona la tecla **Ctrl + Clic** sobre el enlace para abrirlo en una nueva pestaña y no perder este manual).*

https://filezilla-project.org/download.php

Luego de descargar, instala y abre el programa.

## 6. Transferencia y Verificación en Windows 🪟 (o MacOS)

Un respaldo no sirve de nada si no puedes recuperar la información. Tu tarea final es extraer el archivo generado y verificar su contenido en tu computadora personal.

### Paso 1: Conexión mediante FileZilla
Abre FileZilla en tu computadora. En la barra superior de "Conexión rápida", llena los campos exactamente así:

* **Servidor:** Escribe `sftp://` seguido de la IP de tu servidor Linux (Ejemplo: `sftp://192.168.1.50`).
* **Nombre de usuario:** Tu usuario de Linux (Ejemplo: `root` o tu usuario administrador).
* **Contraseña:** La contraseña de tu servidor.
* **Puerto:** Escribe `22` (Este paso es obligatorio para conectarse por SFTP de forma segura).
* Haz clic en **Conexión rápida**.

### Paso 2: Descarga del respaldo
1. En el panel derecho (Sitio remoto), navega hasta la ruta: `/ejercicio/almacenamiento`.
2. Busca el archivo con extensión `.zip` que generó tu script.
3. En el panel izquierdo (Sitio local), elige la carpeta de tu computadora donde quieres guardarlo.
4. Haz doble clic en el archivo `.zip` (panel derecho) para que comience la descarga.

### Paso 3: Verificación final
1. En tu computadora, ve a la carpeta donde descargaste el archivo.
2. Dale clic derecho y selecciona **Extraer todo...** (o doble clic si usas MacOS).
3. Verifica que dentro de la carpeta extraída aparezcan los archivos originales: `archivo1.txt` y `archivo2.txt`.

> **Recuerda:** Deberás sacar una captura de pantalla donde demuestres que lograste este último paso y subirla a la tarea en ClassRoom.


