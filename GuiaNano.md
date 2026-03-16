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
