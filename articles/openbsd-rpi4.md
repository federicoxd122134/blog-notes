---
publishDate: 'May 27 2024'
title: 'Installing OpenBSD on Raspberry Pi4'
description: 'How to install OpenBSD on Raspberry Pi4 in 2024 without using a serial adapter'
image: '~/assets/images/openbsd-rpi.png'
tags: [linux]
---


Vamos a usar `wget` para descargar OpenBSD. Primero, asegúrate de tener `wget` instalado. Si no lo tienes, instálalo con:
```sh
sudo apt-get install wget   # Para Debian/Ubuntu
sudo yum install wget       # Para CentOS/RHEL
sudo pacman -S wget         # Para Arch Linux
```

Ahora, descargamos la imagen de OpenBSD, que al momento de escribir éste artículo se encuentra en la versión 7.5:
```sh
wget https://cdn.openbsd.org/pub/OpenBSD/7.5/arm64/install75.img
```

Por último, verificamos la instalación (puedes saltarte éste paso si lo deseas)
```sh
wget https://cdn.openbsd.org/pub/OpenBSD/7.5/arm64/SHA256
sha256sum -c --ignore-mising SHA256
```
Si el output es `install75.img: OK`, entonces no ha habido ningún error.

---

Usaremos la versión del bootloader EDKII compilada para RPi4 (al momento de escribir éste artículo se encuentra en la versión 1.37). La descargaremos de la siguiente manera:
```sh
wget https://github.com/pftf/RPi4/releases/download/v1.37/RPi4_UEFI_Firmware_v1.37.zip
```

---

Ahora copiaremos la imágen de OpenBSD 7.5 en el pendrive:
```sh
sudo dd if=install75.img of=/dev/sdX bs=1M status=progress
```

Donde /dev/sdX es el archivo que hace referencia al pendrive. Hay que tener **extremo cuidado** con la letra que se elige, ya que `dd` sobreescribirá el dispositivo. Usa `lsblk` para comprobar que se ha elegido el archivo correcto.

Además, borraremos los datos de la tarjeta microSD en la que se instalará OpenBSD. Como dije anteriormente, tener mucho cuidado con el archivo del dispositivo:
```sh
sudo dd if=/dev/zero of=/dev/sdX bs=1M status=progress
```

---
Ahora reemplazaremos la partición de boot con U-Boot en el pendrive por EDK II. Para ello primero montamos la partición de boot:
```sh
* la monta *
```

Luego eliminamos todos los archivos **excepto** la carpeta `efi`. Ésto lo haremos moviendo la carpeta a /tmp y eliminamos todos los archivos en el punto de montaje. Luego movemos la carpeta efi a su directorio original.

```sh
sudo mv /mnt/efi /tmp
sudo rm -fr /mnt/*
sudo mv /tmp/efi /mnt
```

Por último extraemos los contenidos del archivo zip que contiene el EDK II en el directorio montado. Por último desmontamos el dispositivo:
```sh
sudo unzip -x /tmp/UEFI-1.37.zip -d /mnt
sudo umount /mnt
```

---
---
---

Ésta es la mejor parte, la instalación de OpenBSD. Primero conectamos el pendrive con el instalador, un teclado, el cable HDMI y un cable Ethernet. Ya conectado todo encendemos la raspeberry pi4. Cuando aparezca la imagen de raspberry, presiona la tecla `<esc>`. Ésto hará que el bootloader ingrese a una pantalla similar a la BIOS. Puede tardar mucho o poco dependiendo los dispositivos utilizados, así que hay que estar atentos.

Conectamos la tarjeta microSD en la que instalaremos OpenBSD. Hacerlo de esta forma nos garantiza que el proceso de arranque comience desde el dispositivo que tiene el firmware EDK II.

En la pantalla del BIOS, seleccionamos la opción `Boot Manager` y luego el pendrive que contiene la imágen de instalación.

---
Ahora realizaremos la instalación de OpenBSD en la microSD. Al iniciar, nos encontraremos con un prompt y elegiremos la opción de `install`. El proceso de instalación es bastante sencillo. Hay 2 problemas que pueden surgir:

* Tener cuidado con los discos. Revisa con detenimiento el disco donde se va a instalar el sistema operativo para no sobreescribir el pendrive que se está utilizando.
* Si hay un error al seleccionar repositorios, mi solución fue cambiar la cdn de donde se descargarán los paquetes.

Al final de la instalación, nos encontraremos con éste prompt. Elegiremos la opción `(S)hell`:
```txt
Exit to (S)hell, (H)alt or (R)eboot?
```

Ahora ejecutamos el siguiente comando para apagar la raspberry pi4:
```sh
halt -p
```

---

Debido a que hacer la copia del bootloader desde la raspberry no me funcionó, la realicé desde la PC.