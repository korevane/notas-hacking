# Milkslap


[🥛](http://mercury.picoctf.net:48380/)

Look at the problem category: Forensics, Medium, picoCTF 2021

1. Visitamos el link
```
http://mercury.picoctf.net:48380/
```
Se trata de una página con nada más que un gif de un hombre al que le derraman un vaso de leche en la cara.

2. Descargamos la imagen
```
wget http://mercury.picoctf.net:48380/concat_v.png
```

3. Usamos ``zstag`` para la esteganografía de bits menos significativos. Obtenemos la bandera
```
zsteg -s all *.png

picoCTF{imag3_m4n1pul4t10n_sl4p5}
```


**Referencias**
- [Video](https://www.youtube.com/watch?v=lnDj51mO_BI)
- [Herramientas](https://github.com/JohnHammond/ctf-katana#steganography)
- [Esteganografía](https://latam.kaspersky.com/resource-center/definitions/what-is-steganography)



# Disk, disk, sleuth!

Use `srch_strings` from the sleuthkit and some terminal-fu to find a flag in this disk image: [dds1-alpine.flag.img.gz](https://mercury.picoctf.net/static/a734f18939e0aaea9d27bc7a243a0ed0/dds1-alpine.flag.img.gz)

Have you ever used `file` to determine what a file was?
Relevant terminal-fu in picoGym: https://play.picoctf.org/practice/challenge/85
Mastering this terminal-fu would enable you to find the flag in a single command: https://play.picoctf.org/practice/challenge/48
Using your own computer, you could use qemu to boot from this disk!

1. Descargamos el archivo
```
wget https://mercury.picoctf.net/static/a734f18939e0aaea9d27bc7a243a0ed0/dds1-alpine.flag.img.gz
```

2. Descomprimimos el archivo
```
gunzip dds1-alpine.flag.img.gz
```

3. Revisamos el archivo extraído
```
file dds1-alpine.flag.img   

dds1-alpine.flag.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0x10,81,1), startsector 2048, 260096 sectors
```
Dice que es una partición del sector de arranque MBR.

4. Usamos cadenas de búsqueda. Obtenemos la bandera
```
srch_strings dds1-alpine.flag.img | grep pico
ffffffff81399ccf t pirq_pico_get
ffffffff81399cee t pirq_pico_set
ffffffff820adb46 t pico_router_probe
  SAY picoCTF{f0r3ns1c4t0r_n30phyt3_a69a712c}
```


**Referencias:**
- [Video](https://www.youtube.com/watch?v=8vGqrtl87JI)


# Sleuthkit Intro

Download the disk image and use `mmls` on it to find the size of the Linux partition. Connect to the remote checker service to check your answer and get the flag. Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory. [Download disk image](https://artifacts.picoctf.net/c/164/disk.img.gz) Access checker program: `nc saturn.picoctf.net 49940`

1. Descargamos la imagen
```
wget https://artifacts.picoctf.net/c/164/disk.img.gz 
```

2. Descomprimimos
```
gunzip disk.img.gz
```

3. Revisamos el archivo y vemos que es una partición de arranque MBR.
```
file disk.img

disk.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0xc,190,50), startsector 2048, 202752 sectors
```

4. Se nos indició que ejecutáramos el comando ``mmls`` en la imagen. Es un comando que nos dice cuál es el contenido de una imagen de disco
```
mmls disk.img                     

DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000204799   0000202752   Linux (0x83)
```
El tamaño de la partición de Linux es 202752

5. Nos conectamos al puerto. Obtenemos la bandera
```
nc saturn.picoctf.net 49940

What is the size of the Linux partition in the given disk image?
Length in sectors: 202752
202752
Great work!
picoCTF{mm15_f7w!}
```


**Referencias:**
- [Video](https://www.youtube.com/watch?v=SuyUQQoFop4)



# Sleuthkit Apprentice

Download this disk image and find the flag. Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.

- [Download compressed disk image](https://artifacts.picoctf.net/c/138/disk.flag.img.gz)

1. Descargamos el archivo
```
wget https://artifacts.picoctf.net/c/138/disk.flag.img.gz
```

2. Descomprimimos
```
gunzip disk.flag.img.gz
```

3. Revisamos el archivo
```
file disk.flag.img 

disk.flag.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0xc,223,19), startsector 2048, 204800 sectors; partition 2 : ID=0x82, start-CHS (0xc,223,20), end-CHS (0x16,111,25), startsector 206848, 153600 sectors; partition 3 : ID=0x83, start-CHS (0x16,111,26), end-CHS (0x26,62,24), startsector 360448, 253952 sectors
```
Es un archivo de sector de arranque NBR.

4. Recuperamos las particiones. Hay 3
```
mmls disk.flag.img

DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000360447   0000153600   Linux Swap / Solaris x86 (0x82)
004:  000:002   0000360448   0000614399   0000253952   Linux (0x83)
```

5. Enumeramos las particiones para obtener más información
```
fsstat -o 2048 disk.flag.img

FILE SYSTEM INFORMATION
--------------------------------------------
File System Type: Ext4
Volume Name: 
Volume ID: 8e023955b4e7dab7e04b7643076ccf0f
```
Nos da mucha información, pero lo importante es que obtenemos el tipo de sistema de archivo que está usando: Ext4.

```
fsstat -o 360448 disk.flag.img

FILE SYSTEM INFORMATION
--------------------------------------------
File System Type: Ext4
Volume Name: 
Volume ID: 3c054136f02898b3224bd632cbd6c255
```

En la partición 206848 no se puede determinar el sistema de archivos, así que lo omitimos.

6. Listamos los archivos ext4
```
fls -f ext4 -o 2048 -r disk.flag.img 

d/d 11: lost+found
r/r 12: ldlinux.sys
r/r 13: ldlinux.c32
r/r 15: config-virt
r/r 16: vmlinuz-virt
r/r 17: initramfs-virt
l/l 18: boot
r/r 20: libutil.c32
r/r 19: extlinux.conf
r/r 21: libcom32.c32
r/r 22: mboot.c32
r/r 23: menu.c32
r/r 14: System.map-virt
r/r 24: vesamenu.c32
V/V 25585:      $OrphanFiles
```
Nada interesante

```
fls -f ext4 -o 360448 -r disk.flag.img 

... Muchos archivos
d/d 1992:       media
+ d/d 457:      cdrom
+ d/d 458:      floppy
+ d/d 459:      usb
d/d 1993:       mnt
d/d 1994:       opt
d/d 1995:       root
+ r/r 2363:     .ash_history
+ d/d 3981:     my_folder
++ r/r * 2082(realloc): flag.txt
++ r/r 2371:    flag.uni.txt
d/d 1996:       run
d/d 1997:       srv
d/d 1998:       sys
d/d 2358:       swap
V/V 31745:      $OrphanFiles
```
Encontramos el archivo con la bandera. Guardamos los números de ID

7. Leemos los archivos
```
icat -f ext4 -o 360448 disk.flag.img 2082

3.449677            13.056403
```
No hay bandera

```
icat -f ext4 -o 360448 disk.flag.img 2371

picoCTF{by73_5urf3r_2f22df38}
```
Obtenemos la bandera


**Referencias:**
- [Video](https://www.youtube.com/watch?v=1sFEUl8UpM0)



# Operation Orchid

Download this disk image and find the flag. Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.

- [Download compressed disk image](https://artifacts.picoctf.net/c/212/disk.flag.img.gz)

1. Descargamos el archivo
```
wget https://artifacts.picoctf.net/c/212/disk.flag.img.gz
```

2. Descomprimimos
```
gunzip disk.flag.img.gz
```

3. Revisamos el archivo
```
file disk.flag.img    

disk.flag.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0xc,223,19), startsector 2048, 204800 sectors; partition 2 : ID=0x82, start-CHS (0xc,223,20), end-CHS (0x19,159,6), startsector 206848, 204800 sectors; partition 3 : ID=0x83, start-CHS (0x19,159,7), end-CHS (0x32,253,11), startsector 411648, 407552 sectors
```

4. Revisamos las particiones
```
mmls disk.flag.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000411647   0000204800   Linux Swap / Solaris x86 (0x82)
004:  000:002   0000411648   0000819199   0000407552   Linux (0x83)
```

5. Revisamos las particiones
```
fls disk.flag.img -o 2048

d/d 11: lost+found
r/r 12: ldlinux.sys
r/r 13: ldlinux.c32
r/r 15: config-virt
r/r 16: vmlinuz-virt
r/r 17: initramfs-virt
l/l 18: boot
r/r 20: libutil.c32
r/r 19: extlinux.conf
r/r 21: libcom32.c32
r/r 22: mboot.c32
r/r 23: menu.c32
r/r 14: System.map-virt
r/r 24: vesamenu.c32
V/V 25585:      $OrphanFiles
```

```
fls disk.flag.img -o 411648

d/d 460:        home
d/d 11: lost+found
d/d 12: boot
d/d 13: etc
d/d 81: proc
d/d 82: dev
d/d 83: tmp
d/d 84: lib
d/d 87: var
d/d 96: usr
d/d 106:        bin
d/d 120:        sbin
d/d 466:        media
d/d 470:        mnt
d/d 471:        opt
d/d 472:        root
d/d 473:        run
d/d 475:        srv
d/d 476:        sys
d/d 2041:       swap
V/V 51001:      $OrphanFiles
```

6. Revisamos el directorio raíz
```
fls disk.flag.img 472 -o 411648

r/r 1875:       .ash_history
r/r * 1876(realloc):    flag.txt
r/r 1782:       flag.txt.enc
```

7. Revisamos los archivos txt
```
icat -o 411648 disk.flag.img 1876

-0.881573            34.311733
```

```
icat -o 411648 disk.flag.img 1782
Salted__0��!�-6V����0��U��l��&�:�pj_1�0�|�h�Ȥ7� ���؎$�'%       
```
Parece que encontramos la bandera pero está encriptada

8. Revisamos el archivo de historial ascore
```
icat -o 411648 disk.flag.img 1875

touch flag.txt
nano flag.txt 
apk get nano
apk --help
apk add nano
nano flag.txt 
openssl
openssl aes256 -salt -in flag.txt -out flag.txt.enc -k unbreakablepassword1234567
shred -u flag.txt
ls -al
halt
```
Obtenemos el historial de la consola para esta partición, podemos decodificar la bandera usando OPENSSL

9. Enviamos la bandera a un archivo txt
```
icat -o 411648 disk.flag.img 1782 > flag.txt.enc
```

10. Decodificamos usando OPENSSL
```
openssl aes256 -d -salt -in flag.txt.enc -out flag.txt -k unbreakablepassword1234567
```

11. Obtenemos la bandera
```
cat flag.txt

picoCTF{h4un71ng_p457_0a710765}  
```

**Referencias:**
- [Video](https://youtu.be/rXY4BCtonJs?si=HXBc46bWw8ydJ2w_)



# Operation Oni

Download this disk image, find the key and log into the remote machine. Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.

- [Download disk image](https://artifacts.picoctf.net/c/71/disk.img.gz)
- Remote machine: `ssh -i key_file -p 55465 ctf-player@saturn.picoctf.net`

1. Descargamos el archivo
```
wget https://artifacts.picoctf.net/c/71/disk.img.gz  
```

2. Descomprimimos
```
gunzip disk.img.gz  
```

3. Revisamos las particiones
```
mmls disk.img    

DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000471039   0000264192   Linux (0x83)
```

4. Revisamos internamente las particiones
```
fls -o 206848 disk.img 

d/d 458:        home
d/d 11: lost+found
d/d 12: boot
d/d 13: etc
d/d 79: proc
d/d 80: dev
d/d 81: tmp
d/d 82: lib
d/d 85: var
d/d 94: usr
d/d 104:        bin
d/d 118:        sbin
d/d 464:        media
d/d 468:        mnt
d/d 469:        opt
d/d 470:        root
d/d 471:        run
d/d 473:        srv
d/d 474:        sys
V/V 33049:      $OrphanFiles
```

```
fls -o 206848 disk.img 470

r/r 2344:       .ash_history
d/d 3916:       .ssh
```

```
fls -o 206848 disk.img 3916

r/r 2345:       id_ed25519
r/r 2346:       id_ed25519.pub
```
Parecen archivos sospechosos

```
icat -o 206848 disk.img 2345

-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBgrXe4bKNhOzkCLWOmk4zDMimW9RVZngX51Y8h3BmKLAAAAJgxpYKDMaWC
gwAAAAtzc2gtZWQyNTUxOQAAACBgrXe4bKNhOzkCLWOmk4zDMimW9RVZngX51Y8h3BmKLA
AAAECItu0F8DIjWxTp+KeMDvX1lQwYtUvP2SfSVOfMOChxYGCtd7hso2E7OQItY6aTjMMy
KZb1FVmeBfnVjyHcGYosAAAADnJvb3RAbG9jYWxob3N0AQIDBAUGBw==
-----END OPENSSH PRIVATE KEY-----
```
Parece una llave

5. Guardamos la llave
```
icat -o 206848 disk.img 2345 > key_file
```

```
chmod 400 key_file
```

6. Nos conectamos al puerto usando la llave. Obtenemos la bandera
```
ssh -i key_file -p 55465 ctf-player@saturn.picoctf.net

ctf-player@challenge:~$ ls
flag.txt
ctf-player@challenge:~$ cat flag.txt
picoCTF{k3y_5l3u7h_af277f77}ctf-player@challenge:~$ 
```


**Referencias:**
- [Video](https://www.youtube.com/watch?v=PVeV-S3Zbqk)
- [sleuthkit]([https://www.sleuthkit.org/index.php](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqazlZS0hObkM4RFFXdWk4RFJTZFRHQWM4UkxoZ3xBQ3Jtc0tsODdEVi0ta00tQk10akRfV3d0aGZtRnMyUUQ0Vk9fMnNGVzF1NDRkZHVqSHJwcDM1WlljRTlvVzJWRENZZURRMF9vaE9jaDJxbE1VbnFPNW1Mb1NCX3NhN0dDM1picUN6WjA1QVo5bTlqN25WclZ6UQ&q=https%3A%2F%2Fwww.sleuthkit.org%2Findex.php&v=PVeV-S3Zbqk))