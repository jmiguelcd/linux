# Manual de Nano en Linux 🐧

## 1. Crear una carpeta de trabajo y crear un archivo nuevo con Nano
El comando mkdir, es para crear una carpeta.
Para abrir o crear un archivo, basta con llamar a nano.

Copia y pega estos comandos en terminal:
```
sudo mkdir /ejercicio
sudo nano /ejercicio/ejercicio1.sh
```

Una vez dentro, pega este texto.
```
#!/bin/bash
echo "Hola Mundo nano!"
```

## 2. Salir y guardar archivo
Debes presionar la combibacion de tecas:
` CTRL + x `

Luego, confirmas que quieres guardar con ` y `

Te preguntará donde guardar, por defecto te muestra la misma ubicación que abriste. Confirma presionando ` ENTER `.

## 3. Verificar y ejecuta el archivo

Para verificar el contenido del archivo usaremos ` cat `
```
cat /ejercicio/ejercicio1.sh
```

Si grabaste correctamente, deberás ver esto:
```
cat /ejercicio/ejercicio1.sh
#!/bin/bash
echo "Hola Mundo nano!"
```

Ahora, puedes ejecutar el archivo con este comando:
```
sudo sh /ejercicio/ejercicio1.sh
```

El comando ` echo ` que utilizamos, sirve para imprimir en la pantalla un texto que desees mostrar.

## 4. Atajos útiles en Nano

Para editar tus scripts de forma más rápida, usa estos comandos dentro de Nano:

* **Buscar texto:** `CTRL + W` (Escribe la palabra y presiona ENTER).
* **Cortar una línea completa:** `CTRL + K`.
* **Copiar una línea completa (sin borrarla):** `ALT + 6` (En Mac: presiona `ESC`, suelta, y luego el número `6`).
* **Pegar la línea (donde esté el cursor):** `CTRL + U`.
* **Ir al inicio de la línea:** `CTRL + A`.
* **Ir al final de la línea:** `CTRL + E`.
* **Deshacer el último cambio:** `ALT + U` (En Mac: presiona `ESC`, suelta, y luego `U`).
* **Rehacer:** `ALT + E` (En Mac: presiona `ESC`, suelta, y luego `E`).
* **Ir al inicio del archivo:** `ALT + \` (En Mac: presiona `ESC`, suelta, y luego la tecla `\`).
* **Ir al final del archivo:** `ALT + /` (En Mac: presiona `ESC`, suelta, y luego la tecla `/`).

## 5. Uso de comentarios en tus scripts

Los comentarios son notas que el sistema ignora al ejecutar el script, pero que son vitales para que tú (o tus colegas) entiendan qué hace cada parte del código. Esto es fundamental para documentar y para auditorías.

* **¿Cómo ponerlos?:** Usa el símbolo de gato `#`. Todo lo que escribas después de ese símbolo en la misma línea será ignorado por Linux.

**Ejemplo de cómo se ve en el código:**

```bash
#!/bin/bash
# Este es un comentario: El siguiente comando saluda al usuario
echo "Hola, clase"

# Puedes comentar líneas completas para "desactivar" comandos sin borrarlos
# echo "Este comando no se ejecutará"
```

## 6. El famoso "Shebang" (#!)
Todos tus scripts deben empezar con esta línea en el renglón número 1: `#!/bin/bash`. 

* **¿Para qué sirve?:** Le dice al sistema operativo qué programa debe usar para leer el archivo. 
* **Importancia:** Esto asegura que el script se ejecute siempre en el entorno correcto (Bash) y no cause errores inesperados en otros intérpretes.

## 7. Variables y el comando de Fecha
Las variables son como "cajitas" donde guardamos información para usarla después. En Bash, es regla de oro **no dejar espacios** antes ni después del signo `=`.

Por ejemplo, vamos a guardar la fecha y hora en una variable usando la fecha del sistema. Vamos a editar el archivo que ya tienes para que ahora nos diga el día actual.

### Paso 1: Abre tu archivo con Nano
```bash
sudo nano /ejercicio/ejercicio1.sh
```

### Paso 2: Edita el contenido para que quede así:
```bash
#!/bin/bash
echo "Hola Mundo nano!"

# Guardamos el día actual en una variable llamada HOY
HOY=$(date +%d)

# Para llamar a la variable y mostrar su contenido, usamos el signo $
echo "Hoy es el día $HOY del mes."
```

### Paso 3: Salir y guardar
1. Presiona `CTRL + X` para salir.
2. Confirma que quieres guardar con la tecla `y`.
3. Presiona `ENTER` para confirmar el mismo nombre de archivo.

### Paso 4: Verifica y ejecuta
```bash
sudo sh /ejercicio/ejercicio1.sh
```

## 8. Ajustar la zona horaria (México)
Antes de programar tareas automáticas, debemos asegurar que la computadora tenga la hora correcta de nuestra región. De lo contrario, tus scripts podrían ejecutarse a una hora que no esperas.

* **Comando para ver la hora actual:** `date`
* **Comando para ajustar a Ciudad de México:**
```bash
sudo timedatectl set-timezone America/Mexico_City
```

## 9. Permisos de ejecución (chmod)
Por seguridad, Linux crea los archivos nuevos solo para ser leídos o editados, pero no para ser "corridos" como programas. Si intentas ejecutar tu script sin este paso, verás el error: `Permission denied`.

* **El comando:** `sudo chmod +x /ejercicio/ejercicio1.sh`
* **¿Qué hace?:** La `+x` significa "más ejecución" (executable). Esto le otorga al archivo el permiso de funcionar como un programa real del sistema.

Una vez que el archivo tiene permisos, ya puedes ejecutarlo directamente escribiendo la ruta:
```bash
sudo /ejercicio/ejercicio1.sh
```

## 10. Formato avanzado de Fecha y Hora
Es vital que manejemos sellos de tiempo (timestamp) cuando creamos scripts personalizados. Vamos a aprender a darle un formato profesional: ` AÑO MES DIA HORA MINUTO SEGUNDO `.

### Los códigos que usaremos:
* **%Y**: Año con 4 dígitos (2026).
* **%m**: Mes con número (01 a 12).
* **%d**: Día del mes (01 a 31).
* **%H**: Hora en formato 24 horas (00 a 23).
* **%M**: Minutos (00 a 59).
* **%S**: Segundos (00 a 59).

### Paso 1: Edita tu script con Nano
Vamos a actualizar la variable para que use este nuevo formato.
```bash
sudo nano /ejercicio/ejercicio1.sh
```

### Paso 2: Cambia el contenido
Modifica tu script para que se vea así (fíjate que no hay espacios después del signo `+`):
```bash
#!/bin/bash
echo "Hola Mundo nano!"

# Guardamos el día actual en una variable llamada HOY
HOY=$(date +%d)

# Para llamar a la variable y mostrar su contenido, usamos el signo $
echo "Hoy es el día $HOY del mes."

# Guardamos la fecha completa: Año-Mes-Día-Hora-Min-Seg
SelloTiempo=$(date +%Y%m%d%H%M%S)

echo "El sello de tiempo es: $SelloTiempo"
```

### Paso 3: Guarda y ejecuta
1. Presiona `CTRL + X`, luego `y` y al final `ENTER`.
2. Ejecuta el script:
```bash
sudo sh /ejercicio/ejercicio1.sh
```

> **Por qué importa:** Si ejecutas el script varias veces, verás que el número siempre cambia. Esto evita que un respaldo nuevo sobreescriba a una anterior.


