
# m00nwalk

Decode this [message](https://jupiter.challenges.picoctf.org/static/d6fcea5e3c6433680ea4f914e24fab61/message.wav) from the moon.

How did pictures from the moon landing get sent back to Earth?
What is the CMU mascot?, that might help select a RX option

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/d6fcea5e3c6433680ea4f914e24fab61/message.wav
```

2. Examinamos el archivo. Vemos que es un archivo de audio
```
file message.wav

message.wav: RIFF (little-endian) data, WAVE audio, Microsoft PCM, 16 bit, mono 48000 Hz
```

3. Aplicamos strings sobre el archivo para ver si no hay algo escondido. Parece que no hay nada
```
strings -n 10 message.wav 

$]&,&$$r ^
LSU3X0UXL)>
AK@F<45f+E
AFG+HUD-<_0
<_GsMpNXJ}Aw4
...
```

4. Abrimos el archivo. No parece ser la gran cosa, sólo algunos sonidos que parecen pertenecer a una nave espacial
```
open message.wav
```

5. Puede que el hubiera una imagen escondida en el audio. Usamos un decodificador de github para extraer la imagen
```
clonamos el repositorio: 
git clone https://github.com/colaclanth/sstv.git

instalamos los paquetes:
sudo python setup.py install

ejecutamos el programa:
sstv -d message.wav -o result.png
```
Como resultado obtenemos una imagen invertida con algo que podría ser una bandera. 

6. Rotamos la imagen y obtenemos la bandera
```
picoCTF{beep_boop_im_in_space}
```


**Referencias:**
- [Video](https://www.youtube.com/watch?v=UyLTEpAz6eE)
- [apolo 11 - sstv](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbThhSDIxTFBTSDlwM3c4T2hvczVnbDNUb1lCd3xBQ3Jtc0tsWjdLTWllZjF3STdfM05BendXMlQ2TmszZkRDMWxnNldXazhWUUdEZW9nNk9YRjlna0oxNFIyYXpVX2ZocmpwUTBSUWtXc1pIWU1HRjV6M0xkX0xPUUNibDR6NTgzT2xrUVNsZ014QTZRODFtdmNnNA&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FApollo_11_missing_tapes&v=UyLTEpAz6eE)
- [sstv decoder git repo](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3JBbnR4ajVCUndGUzVYX3dDMXRncEYtUGw2QXxBQ3Jtc0ttZWZSS0FMU3piaHlTZDdPNzY4T0JEbk93eFZEeXRRWTlBSG5kbnF4NUprZVVSX29YNEU1aXJMUUc0OEYzcHo3VEV6cXN3dzFNUDhXNExCSkR4b1QtYndLbHlhUVhvMml0WnJRV0JWZC1kTVRPcVN1RQ&q=https%3A%2F%2Fgithub.com%2Fcolaclanth%2Fsstv&v=UyLTEpAz6eE)


# WhitePages

I stopped using YellowPages and moved onto WhitePages... but [the page they gave me](https://jupiter.challenges.picoctf.org/static/74274b96fe966126a1953c80762af80d/whitepages.txt) is all blank!

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/74274b96fe966126a1953c80762af80d/whitepages.txt
```

2. Como es un ``.txt`` intentamos hacer un ``cat``
```
cat whitepages.txt 









```
Obtenemos sólo espacio en blanco.

3. Revisamos la longitud del archivo y comprobamos que sí hay algo
```
ls -la

-rw-rw-r--  1 sailork sailork  2964 oct 26  2020  whitepages.txt
```
Tiene un longitud de 2964

4. Contamos los caracteres
```
wc whitepages.txt

0    0 2964 whitepages.txt
```
No hay ni una sola línea escrita, pero tiene 2964 caracteres

5. Revisamos qué tipo de archivo es
```
file whitepages.txt

whitepages.txt: Unicode text, UTF-8 text, with very long lines (1376), with no line terminators
```
Vemos que está en el estándar Unicode UTF-8

6. Mostramos en hexadecimal los valores
```
xxd whitepages.txt | head

00000000: e280 83e2 8083 e280 83e2 8083 20e2 8083  ............ ...
00000010: 20e2 8083 e280 83e2 8083 e280 83e2 8083   ...............
00000020: 20e2 8083 e280 8320 e280 83e2 8083 e280   ...... ........
00000030: 83e2 8083 20e2 8083 e280 8320 e280 8320  .... ...... ... 
00000040: 2020 e280 83e2 8083 e280 83e2 8083 e280    ..............
00000050: 8320 20e2 8083 20e2 8083 e280 8320 e280  .  ... ...... ..
00000060: 8320 20e2 8083 e280 83e2 8083 2020 e280  .  .........  ..
00000070: 8320 20e2 8083 2020 2020 e280 8320 e280  .  ...    ... ..
00000080: 83e2 8083 e280 83e2 8083 2020 e280 8320  ..........  ... 
00000090: e280 8320 e280 8320 e280 83e2 8083 e280  ... ... ........
```
e28083 se repite varias veces, un carácter que representa un espacio en blanco.
20 es código hexadecimal del espacio tradicional.
Hay dos tipos de espacios. Uno de ellos se conforma por 3 caracteres Unicode, y otro que sólo utiliza 1.
Posiblemente sea código binario oculto.

7. Debemos convertir todos los e28083 por 0, y todos los 20 por 1. Usamos la librería de python pwntools
```
nano whitepages_python.py
```

```python
from pwn import *

# abrir el archivo en lectura binaria
file = open("whitepages.txt", "rb")

# convierte el archivo en un objeto bytearray para facilitar la manipulación de datos
data = bytearray(file.read())

# reemplazar \xe2\x80\x83 por 0
data = data.replace(b'\xe2\x80\x83', b'0')

# reemplazar \x20 por 1
data = data.replace(b'\x20', b'1')

print(data)
```

```
bytearray(b'00001010000010010000100101110000011010010110001101101111010000110101010001000110000010100000101000001001000010010101001101000101010001010010000001010000010101010100001001001100010010010100001100100000010100100100010101000011010011110101001001000100010100110010000000100110001000000100001001000001010000110100101101000111010100100100111101010101010011100100010000100000010100100100010101010000010011110101001001010100000010100000100100001001001101010011000000110000001100000010000001000110011011110111001001100010011001010111001100100000010000010111011001100101001011000010000001010000011010010111010001110100011100110110001001110101011100100110011101101000001011000010000001010000010000010010000000110001001101010011001000110001001100110000101000001001000010010111000001101001011000110110111101000011010101000100011001111011011011100110111101110100010111110110000101101100011011000101111101110011011100000110000101100011011001010111001101011111011000010111001001100101010111110110001101110010011001010110000101110100011001010110010001011111011001010111000101110101011000010110110001011111011000110011010100110100011001100011001000110111011000110110010000110000001101010110001100110010001100010011100000111001011001100011100000110001001101000011011101100011011000110011011001100110001101010110010001100101011000100011001001100101001101010011011001111101000010100000100100001001')
```

8. Modificamos el código para convertir de binario a ASCII
```python
from pwn import *

# abrir el archivo en lectura binaria
file = open("whitepages.txt", "rb")

# convierte el archivo en un objeto bytearray para facilitar la manipulación de datos
data = bytearray(file.read())

# reemplazar \xe2\x80\x83 por 0
data = data.replace(b'\xe2\x80\x83', b'0')

# reemplazar \x20 por 1
data = data.replace(b'\x20', b'1')

# convertir los datos procesados de bytes a una cadena de texto en ascii
data = data.decode('ascii') 

# toma una cadena binaria y la interpreta como bits 
data = unbits(data)

print(data)
```

8. Obtenemos la bandera
```
python whitepages_python.py

b'\n\t\tpicoCTF\n\n\t\tSEE PUBLIC RECORDS & BACKGROUND REPORT\n\t\t5000 Forbes Ave, Pittsburgh, PA 15213\n\t\tpicoCTF{not_all_spaces_are_created_equal_c54f27cd05c2189f8147cc6f5deb2e56}\n\t\t'
```


**Referencias**
- [Video](https://www.youtube.com/watch?v=427HDV7tzow)
- [unicode](https://en.wikipedia.org/wiki/Unicode)
- [utf-8](https://en.wikipedia.org/wiki/UTF-8)
- [unicode space separator](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbE5kcHltRU9VNnNRQVViT1lKbHVMUmpjYTRLQXxBQ3Jtc0tuaGdsVERZazZEN3hNNEcxQ00zek9IR0ltSTIyQnc0dUpSQ3pwZnZKbnFIOUhXWDdWNDhZYzdld3JBVWNwcUJlcFdrbDhicC1FWUs0cTVqOUhTWVJrQlhJaVFxVjhvVlBkZWpTSVZOdnVrZllaX0tMTQ&q=https%3A%2F%2Fwww.compart.com%2Fen%2Funicode%2Fcategory%2FZs&v=427HDV7tzow)


# c0rrupt

We found this [file](https://jupiter.challenges.picoctf.org/static/ab30fcb7d47364b4190a7d3d40edb551/mystery). Recover the flag.

Try fixing the file header

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/ab30fcb7d47364b4190a7d3d40edb551/mystery   
```

2. Examinamos el archivo
```
file mystery 

output:
mystery: data
```

3. Revisamos los datos en hexadecimal. Notamos que la cabecera del archivo está dañada
```
xxd mystery | head

output:
00000000: 8965 4e34 0d0a b0aa 0000 000d 4322 4452  .eN4........C"DR
00000010: 0000 066a 0000 0447 0802 0000 007c 8bab  ...j...G.....|..
00000020: 7800 0000 0173 5247 4200 aece 1ce9 0000  x....sRGB.......
00000030: 0004 6741 4d41 0000 b18f 0bfc 6105 0000  ..gAMA......a...
00000040: 0009 7048 5973 aa00 1625 0000 1625 0149  ..pHYs...%...%.I
00000050: 5224 f0aa aaff a5ab 4445 5478 5eec bd3f  R$......DETx^..?
00000060: 8e64 cd71 bd2d 8b20 2080 9041 8302 08d0  .d.q.-.  ..A....
00000070: f9ed 40a0 f36e 407b 9023 8f1e d720 8b3e  ..@..n@{.#... .>
00000080: b7c1 0d70 0374 b503 ae41 6bf8 bea8 fbdc  ...p.t...Ak.....
00000090: 3e7d 2a22 336f de5b 55dd 3d3d f920 9188  >}*"3o.[U.==. ..
```

4. Usamos un editor hexadecimal para arreglar el error. Primero suponemos que es un archivo PNG
```
hexeditor mystery
```

```
00000000  89 65 4E 34  0D 0A B0 AA   00 00 00 0D  43 22 44 52     .eN4........C"DR
```

```
00000000  89 50 4E 47  0D 0A 1A 0A   00 00 00 0D  43 22 44 52     .PNG........C"DR
```

5. Revisamos la extensión del archivo, pero sigue siendo datos
```
file mystery 

output:
mystery: data
```

6. Olvidamos modificar también los `chuncks`
```
hexeditor mystery
```

```
89 50 4E 47  0D 0A 1A 0A   00 00 00 0D  49 48 44 52     .PNG........IHDR
```

7. Volvemos a revisar el archivo y ya es reconocido
```
file mystery   

mystery: PNG image data, 1642 x 1095, 8-bit/color RGB, non-interlaced
```

8. Intentamos abrir el archivo pero aún hay errores en el formato del archivo que nos impiden abrirlo. Usamos pngcheck para verificar los errores
```
pngcheck -v mystery
zlib warning:  different version (expected 1.2.13, using 1.3.1)

File: mystery (202940 bytes)
  chunk IHDR at offset 0x0000c, length 13
    1642 x 1095 image, 24-bit RGB, non-interlaced
  chunk sRGB at offset 0x00025, length 1
    rendering intent = perceptual
  chunk gAMA at offset 0x00032, length 4: 0.45455
  chunk pHYs at offset 0x00042, length 9: 2852132389x5669 pixels/meter
  CRC error in chunk pHYs (computed 38d82c82, expected 495224f0)
ERRORS DETECTED in mystery
```
Hay un error en el chunk pHYS, el cual contiene la resolución de la imagen

9. Parece ser una imagen cuadrada
```
00 09 70 48  59 73 AA 00   16 25 00 00  16 25 01 49     ..pHYs...%...%.I
```

```
00 09 70 48  59 73 00 00   16 25 00 00  16 25 01 49     ..pHYs...%...%.I
```

10. El error fue corregido, pero ahora detectamos otro error en el siguiente chunk
```
pngcheck -v mystery

zlib warning:  different version (expected 1.2.13, using 1.3.1)

File: mystery (202940 bytes)
  chunk IHDR at offset 0x0000c, length 13
    1642 x 1095 image, 24-bit RGB, non-interlaced
  chunk sRGB at offset 0x00025, length 1
    rendering intent = perceptual
  chunk gAMA at offset 0x00032, length 4: 0.45455
  chunk pHYs at offset 0x00042, length 9: 5669x5669 pixels/meter (144 dpi)
:  invalid chunk length (too large)
ERRORS DETECTED in mystery
```
Después del pHYS viene el chunk IDAT. Un archivo png puede tener varios chunks IDAT dependiendo de su tamaño

11. Arreglamos el IDAT
```
52 24 F0 AA  AA FF A5 AB   44 45 54 78  5E EC BD 3F     R$......DETx^..?
```

```
52 24 F0 AA  AA FF A5 49   44 41 54 78  5E EC BD 3F     R$.....IDATx^..?
```

12. Volvemos a comprobar
```
pngcheck -v mystery

zlib warning:  different version (expected 1.2.13, using 1.3.1)

File: mystery (202940 bytes)
  chunk IHDR at offset 0x0000c, length 13
    1642 x 1095 image, 24-bit RGB, non-interlaced
  chunk sRGB at offset 0x00025, length 1
    rendering intent = perceptual
  chunk gAMA at offset 0x00032, length 4: 0.45455
  chunk pHYs at offset 0x00042, length 9: 5669x5669 pixels/meter (144 dpi)
:  invalid chunk length (too large)
ERRORS DETECTED in mystery
```
El anterior error del nombre del chuck IDAT fue corregido pero aún tenemos el problema de que el tamaño es muy grande

13. Corregimos el tamaño
```
52 24 F0 AA  AA FF A5 49   44 41 54 78  5E EC BD 3F     R$.....IDATx^..?
```

```
52 24 F0 00  00 FF A5 49   44 41 54 78  5E EC BD 3F     R$.....IDATx^..?
```

14. Comprobamos. Ya no hay errores
```
pngcheck -v mystery
zlib warning:  different version (expected 1.2.13, using 1.3.1)

File: mystery (202940 bytes)
  chunk IHDR at offset 0x0000c, length 13
    1642 x 1095 image, 24-bit RGB, non-interlaced
  chunk sRGB at offset 0x00025, length 1
    rendering intent = perceptual
  chunk gAMA at offset 0x00032, length 4: 0.45455
  chunk pHYs at offset 0x00042, length 9: 5669x5669 pixels/meter (144 dpi)
  chunk IDAT at offset 0x00057, length 65445
    zlib: deflated, 32K window, fast compression
  chunk IDAT at offset 0x10008, length 65524
  chunk IDAT at offset 0x20008, length 65524
  chunk IDAT at offset 0x30008, length 6304
  chunk IEND at offset 0x318b4, length 0
No errors detected in mystery (9 chunks, 96.3% compression).
```

15. Abrimos la imagen
```
open mystery
```

14. Encontramos la bandera
```
picoCTF{c0rrupt10n_1847995}
```


**Referencias:**
- [video](https://www.youtube.com/watch?v=7zY4VkiWbBI)
- [file signatures](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbUJ6cW5JOVVfZFd5Tnh6ekR5T0lqbTdwNDFZQXxBQ3Jtc0tuRzdnMjU4anpQRXVLS3g2Y2Q0UGxEUHJkUjhnLUx5TlJ0N19Xd21rczYzYkdFVnNFWkQyVDBnbmYtMW8zNWtSSUY1RUNicHR0YWFscmdwSXpQM0JBckxLQUNtU1JNa1EyLTNwam9pcWJnVXVpNUdTTQ&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FList_of_file_signatures&v=7zY4VkiWbBI)


# like1000

This [.tar file](https://jupiter.challenges.picoctf.org/static/52084b5ad360b25f9af83933114324e0/1000.tar) got tarred a lot.

Try and script this, it'll save you a lot of time

1. Descargamos el archivo
```
 wget https://jupiter.challenges.picoctf.org/static/52084b5ad360b25f9af83933114324e0/1000.tar
```

2. Este reto consiste en descomprimir un archivo que está comprimido sobre sí mismo varias veces. No es recomendable hacerlo manual
```
tar -xvf 1000.tar
```

Creamos un script en Python para automatizar el proceso
```
nano extraer1000.py  
```

```python
import tarfile
import os

filename = "1000.tar"

while filename.endswith(".tar"):
    with tarfile.open(filename) as tar:
        tar.extractall()
        next_file = tar.getnames()[0]  # Se asume que hay un único archivo dentro
    print(f"Extracted: {filename} → {next_file}")
    os.remove(filename)  # Borra el archivo tar actual
    filename = next_file  # Prepara el nombre del siguiente archivo

print("Proceso completado")
```

3. Ejecutamos el script y obtenemos la bandera en una imagen PNG
```
python extraer1000.py

/home/sailork/extraer1000.py:8: DeprecationWarning: Python 3.14 will, by default, filter extracted tar archives and reject files or modify their metadata. Use the filter argument to control this behavior.
  tar.extractall()
Extracted: 1000.tar → 999.tar
Extracted: 999.tar → 998.tar
Extracted: 998.tar → 997.tar
Extracted: 997.tar → 996.tar
```

```
picoCTF{l0t5_0f_TAR5}
```


**Referencias:**
- [video](https://www.youtube.com/watch?v=AXsQ7OiGCK8)



# shark on wire 2

We found this [packet capture](https://jupiter.challenges.picoctf.org/static/b506393b6f9d53b94011df000c534759/capture.pcap). Recover the flag that was pilfered from the network.

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/b506393b6f9d53b94011df000c534759/capture.pcap
```

2. Abrimos el archivo en WireShark
3. Seleccionamos el primer UDP
4. Click derecho
5. Follow
6. UDP Stream
7. Revisamos los stream. Encontramos algunas banderas falsas
En el stream 32 encontramos 
```
start
```
Va de 10.0.0.66 a 10.0.0.1 con una longitud de 60. Puerto destino 22

En el stream 60 encontramos
```
end
```
Va de 10.0.0.80 a 10.0.0.1 con una longitud de 60. Puerto destino 22

8. Filtramos todos los paquetes cuyo puerto destino sea el 22
```
udp.dstport == 22
```
Vemos que todos los puertos de origen varían alrededor de 5000

9. El primer puerto es el 5000. El segundo puerto es el 5112. Hacemos una prueba para convertir el carácter 112 a ASCII
```
python3
chr(112)
	'p'
chr(105)
	'i'
```
Vemos que hay un patrón. La bandera está escondida en IDs de los puertos

10. Para automatizar el proceso de extracción de la bandera usamos Scapy. Creamos un script en Python
```
nano SharkOnWire2.py
```

```python
from scapy.all import *

# Lee todos los paquetes del archivo 'capture.pcap' y los guarda en la lista 'packets'
packets = rdpcap('capture.pcap')

# Inicializa una cadena vacía donde se irá construyendo el flag
flag = ''

# Itera sobre cada paquete en el archivo pcap
for p in packets:
    # Verifica si el paquete contiene una capa UDP y si el puerto de destino es 22
    if UDP in p and p[UDP].dport == 22:
        # Comprueba si el puerto de origen es mayor que 5000
        if p[UDP].sport > 5000:
            # Convierte (puerto de origen - 5000) a un carácter ASCII y lo añade al flag
            flag += chr(p[UDP].sport - 5000)

# Imprime el flag reconstruido al final
print(flag)
```

11. Ejecutamos el código y obtenemos la bandera
```
python SharkOnWire2.py
picoCTF{p1LLf3r3d_data_v1a_st3g0}
```


**Referencias:**
- [Video](https://youtu.be/WcMl1SvQ6hI?si=onji59WgL4ZSRRFf)
- [Scapy](https://scapy.net/)

