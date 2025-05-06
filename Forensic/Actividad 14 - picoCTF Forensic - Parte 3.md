# WebNet0

We found this [packet capture](https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/capture.pcap) and [key](https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/picopico.key). Recover the flag.

Try using a tool like Wireshark.
How can you decrypt the TLS stream?

1. Descargamos el archivo de captura
```
wget https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/capture.pcap
```

2. Descargamos la llave
```
wget https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/picopico.key
```

3. Abrimos el archivo de captura en Wireshark. Vemos que todo está encriptado, así que usamos la llave para desencriptarlo
```
Edición

Preferencias

Protocolos

Desglozamos y buscamos TLS

RSA Key List

+ (Agregar)

Click derecho sobre el nuevo reenglon

Buscamos y cargamos el archivo .key
```

4. Con el contenido de los paquetes ya desencriptados, ya sólo queda buscar la bandera
```
Edición

Buscar siguiente

Detalles de paquete, Cadena, picoCTF{
```

5. Encontramos la bandera
```
Pico-Flag: picoCTF{nongshim.shrimp.crackers}
```


**Referencias**
- [Video](https://www.youtube.com/watch?v=9uflLPoETOc)
- [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security)



# Webnet1

We found this [packet capture](https://jupiter.challenges.picoctf.org/static/fbf98e695555a2a48fe42c9a245de376/capture.pcap) and [key](https://jupiter.challenges.picoctf.org/static/fbf98e695555a2a48fe42c9a245de376/picopico.key). Recover the flag.

Try using a tool like Wireshark.
How can you decrypt the TLS stream?


1. Descargamos el archivo de captura y el de la llave
```
wget https://jupiter.challenges.picoctf.org/static/fbf98e695555a2a48fe42c9a245de376/capture.pcap

wget https://jupiter.challenges.picoctf.org/static/fbf98e695555a2a48fe42c9a245de376/picopico.key
```

2. Abrimos el archivo de captura en wireshark
```
wireshark capture.pcap
```

3. Usamos la llave para descifrar el contenido de los paquetes
```
Edición

Preferencias

Protocolos

Desglozamos y buscamos TLS

RSA Key List

+ (Agregar)

Click derecho sobre el nuevo reenglon

Buscamos y cargamos el archivo .key
```

4. Vemos que hay 3 paquetes HTTP que se ponen en verde, uno de ellos incluye una imagen
```
Seleccionamos el paquete con la imagen

Edición

Extraer objetos

HTTP

Seleccionamos el objeto con la imagen y la guardamos en disco
```

5. Abrimos la imagen
```
open vulture.jpg
```

6. Aplicamos un strings y obtenemos la bandera
```
strings -n 10 vulture.jpg        
picoCTF{honey.roasted.peanuts}
```


**Referencias**
- [Video](https://www.youtube.com/watch?v=Ym3i79nEHjw)
- [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security)



# Matryoshka doll

Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one? Image: [this](https://mercury.picoctf.net/static/2978e1270538613cd8181c7b0dabe9bd/dolls.jpg)

Wait, you can hide files inside files? But how do you find them?
Make sure to submit the flag as picoCTF{XXXXX}

1. Descargamos el archivo
```
wget https://mercury.picoctf.net/static/2978e1270538613cd8181c7b0dabe9bd/dolls.jpg
```

2. Revisamos la imagen con un binwalk
```
binwalk dolls.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 594 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378942, uncompressed size: 383937, name: base_images/2_c.jpg
651600        0x9F150         End of Zip archive, footer length: 22
```
Vemos que dentro de la imagen parece haber otra imagen dentro

3. Extraemos lo que hay dentro de la imagen
```
binwalk -e dolls.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378942, uncompressed size: 383937, name: base_images/2_c.jpg
```

4. Vemos que se crea una subcarpeta con dos archivos dentro (un archivo zip y otra subcarpeta)
```
cd _dolls.jpg.extracted

cd base_images
```

5. Dentro de base_images hay otra imagen y otra subcarpeta, continuación de \_dolls.jpg.extracted
```
ls

2_c.jpg  _2_c.jpg.extracted
```

6. Repetimos los pasos del 2 al 5 hasta encontrar el archivo con la bandera. En este caso fue hasta la 5 iteración
```
(sailork㉿Kali-Linux)-[~/…/base_images/_3_c.jpg.extracted/base_images/_4_c.jpg.extracted]
└─$ ls                
136DA.zip  flag.txt
```

```
cat flag.txt         
picoCTF{4cf7ac000c3fb0fa96fb92722ffb2a32} 
```


**Referencias:**
- [Video](https://www.youtube.com/watch?v=NkbtA7x5aVI)
- [Matrioshka](https://es.wikipedia.org/wiki/Matrioshka)



# tunn3l_v1s10n

We found this [file](https://mercury.picoctf.net/static/7b2d7c26630e977197022d0af09e3aeb/tunn3l_v1s10n). Recover the flag.

Weird that it won't display right...

1. Modificamos los primeros bytes (bytes mágicos) para darle formato al archivo
```
00000000  42 4D 8E 26  2C 00 00 00   00 00 BA D0  00 00 BA D0     BM.&,...........
```

```
00000000  42 4D 8E 26  2C 00 00 00   00 00 BA D0  00 00 28 00     BM.&,.........(.
```

Ya podemos abrir la imagen, pero las dimensiones de la imagen no coinciden con el tamaño.

2. Modificamos los bytes de las dimensiones
```
00000010  00 00 6E 04  00 00 32 01   00 00 01 00  18 00 00 00     ..n...@.........
```

```
00000010  00 00 6E 04  00 00 40 03  00 00 01 00  18 00 00 00     ..n...@.........
```

3. Obtenemos la imagen completa y podemos ver la bandera
```
picoCTF{qu1t3_a_v13w_2020}
```


**Referencias**
-  [Video](https://www.youtube.com/watch?v=1ucy2G1PIh4)
- [BMP](https://en.wikipedia.org/wiki/BMP_file_format)
- [List of signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)



# MacroHard WeakEdge


I've hidden a flag in this file. Can you find it? [Forensics is fun.pptm](https://mercury.picoctf.net/static/9a7436948cc502e9cacf5bc84d2cccb5/Forensics is fun.pptm)

1. Descargamos el archivo
```
wget https://mercury.picoctf.net/static/9a7436948cc502e9cacf5bc84d2cccb5/Forensics%20is%20fun.pptm
```

2. Revisamos el archivo
```
file Forensics\ is\ fun.pptm 
Forensics is fun.pptm: Microsoft PowerPoint 2007+
```

3. Extraemos los archivos comprimidos
```
unzip Forensics\ is\ fun.pptm      
```

4. Entramos a la subcarpeta PPT, slideMaster, hidden. Encontramos una cadena de caracteres que parece estar cifrada en base 64, así que la desciframos. 
```
echo "Z m x h Z z o g c G l j b 0 N U R n t E M W R f d V 9 r b j B 3 X 3 B w d H N f c l 9 6 M X A 1 f Q" | tr -d " " | base64 -d

flag: picoCTF{D1d_u_kn0w_ppts_r_z1p5}  
```


**Referencias**
- [Video](https://www.youtube.com/watch?v=CsCeOp9PFGs)
- [PPT](https://www.reviversoft.com/es/file-extensions/pptm)