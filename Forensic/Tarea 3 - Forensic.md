# information

wget https://mercury.picoctf.net/static/b4d62f6e431dc8e563309ea8c33a06b3/cat.jpg

Look at the details of the file
Make sure to submit the flag as picoCTF{XXXXX}

1. Descargamos el archivo
```
wget https://mercury.picoctf.net/static/b4d62f6e431dc8e563309ea8c33a06b3/cat.jpg
```

2. Revisamos los metadatos
```
exiftool cat.jpg    

ExifTool Version Number         : 13.00
File Name                       : cat.jpg
Directory                       : .
File Size                       : 878 kB
File Modification Date/Time     : 2021:03:15 12:24:46-06:00
File Access Date/Time           : 2025:05:11 18:56:16-06:00
File Inode Change Date/Time     : 2025:05:11 18:56:16-06:00
File Permissions                : -rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1
```

3. Sospechamos de Current IPTC Digest y License, parecen estar en base 64
```
echo "7a78f3d9cfb1ce42ab5a3aa30573d617" | base64 -d
���w}q��q�6i�Zݦ�Ӟ�w�{      
```

```
echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d

picoCTF{the_m3tadata_1s_modified} 
```



# St3g0

Download this image and find the flag.
- [Download image](https://artifacts.picoctf.net/c/217/pico.flag.png)

We know the end sequence of the message will be `$t3g0`.

1. Descargar el archivo
```
wget https://artifacts.picoctf.net/c/217/pico.flag.png
```

2. Analizamos el archivo 
```
file pico.flag.png  

pico.flag.png: PNG image data, 585 x 172, 8-bit/color RGBA, non-interlaced
```

3. Buscamos datos ocultos, como cadenas ocultas en los bits menos significativos. Obtenemos la bandera
```
zsteg pico.flag.png

b1,r,lsb,xy         .. text: "~__B>VG?G@"
b1,rgb,lsb,xy       .. text: "picoCTF{7h3r3_15_n0_5p00n_a9a181eb}$t3g0"
b1,abgr,lsb,xy      .. text: "E2A5q4E%uSA"
b2,b,lsb,xy         .. text: "AAPAAQTAAA"
b2,b,msb,xy         .. text: "HWUUUUUU"
b3,r,lsb,xy         .. file: gfxboot compiled html help file
b4,r,lsb,xy         .. file: Targa image data (16-273) 65536 x 4097 x 1 +4352 +4369 - 1-bit alpha - right "\021\020\001\001\021\021\001\001\021\021\001"
b4,g,lsb,xy         .. file: 0420 Alliant virtual executable not stripped
b4,b,lsb,xy         .. file: Targa image data - Map 272 x 17 x 16 +257 +272 - 1-bit alpha "\020\001\021\001\021\020\020\001\020\001\020\001"
b4,bgr,lsb,xy       .. file: Targa image data - Map 273 x 272 x 16 +1 +4113 - 1-bit alpha "\020\001\001\001"
b4,rgba,msb,xy      .. file: Applesoft BASIC program data, first line number 8
```



# Redaction gone wrong

Now you DON’T see me. This [report](https://artifacts.picoctf.net/c/84/Financial_Report_for_ABC_Labs.pdf) has some critical data in it, some of which have been redacted correctly, while some were not. Can you find an important key that was not redacted properly?

How can you be sure of the redaction?

1. Descargar el archivo
```
wget https://artifacts.picoctf.net/c/84/Financial_Report_for_ABC_Labs.pdf
```

2. Abrir el archivo
```
open Financial_Report_for_ABC_Labs.pdf
```
Se abre un pdf con secciones censuradas, cubiertas por un rectángulo negro. 

3. Pasamos el cursos para seleccionar por todo el archivo y encontramos la bandera
```
Financial Report for ABC Labs, Kigali, Rwanda for the year 2021.
Breakdown - Just painted over in MS word.
Cost Benefit Analysis
Credit Debit
This is not the flag, keep looking
Expenses from the
picoCTF{C4n_Y0u_S33_m3_fully}
Redacted document.
```



# hideme

Every file gets a flag. The SOC analyst saw one image been sent back and forth between two people. They decided to investigate and found out that there was more than what meets the eye [here](https://artifacts.picoctf.net/c/257/flag.png).


1. Descargar el archivo
```
wget https://artifacts.picoctf.net/c/257/flag.png     
```

2. Revisar el archivo
```
file flag.png     

flag.png: PNG image data, 512 x 504, 8-bit/color RGBA, non-interlaced
```

3. Revisar si hay archivos ocultos
```
binwalk flag.png   

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 512 x 504, 8-bit/color RGBA, non-interlaced
41            0x29            Zlib compressed data, compressed
39739         0x9B3B          Zip archive data, at least v1.0 to extract, name: secret/
39804         0x9B7C          Zip archive data, at least v2.0 to extract, compressed size: 2959, uncompressed size: 3108, name: secret/flag.png
42998         0xA7F6          End of Zip archive, footer length: 22
```
Vemos que hay carpetas comprimidas

4. Extraer las carpetas comprimidas ocultas
```
binwalk -e flag.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
41            0x29            Zlib compressed data, compressed
39739         0x9B3B          Zip archive data, at least v1.0 to extract, name: secret/
39804         0x9B7C          Zip archive data, at least v2.0 to extract, compressed size: 2959, uncompressed size: 3108, name: secret/flag.png

WARNING: One or more files failed to extract: either no utility was found or it's unimplemented
```

5. Revisar las carpetas ya descomprimidas
```
cd _flag.png.extracted

ls
29  29.zlib  9B3B.zip  secret

cd secret      

ls
flag.png

open flag.png
```

6. Abrimos la imagen y encontramos la bandera
```
picoCTF{Hiddinng_An_imag3_within_@n_ima9e_dc2ab58f}
```



# Scan Surprise

I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead? You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_atlas/1/challenge.zip)

The same files are accessible via SSH here: `ssh -p 54322 ctf-player@atlas.picoctf.net` Using the password `83dcefb7`. Accept the fingerprint with `yes`, and `ls` once connected to begin. Remember, in a shell, passwords are hidden!

QR codes are a way of encoding data. While they're most known for storing URLs, they can store other things too.

Mobile phones have included native QR code scanners in their cameras since version 8 (Oreo) and iOS 11

If you don't have access to a phone, you can also use zbar-tools to convert an image to text

1. Descargar el archivo
```
wget https://artifacts.picoctf.net/c_atlas/1/challenge.zip
```

2. Descomprimimos. El archivo se guardo en otra parte que no era la carpeta donde estábamos trabajando. 
```
unzip challenge.zip 
Archive:  challenge.zip
replace home/ctf-player/drop-in/flag.png? [y]es, [n]o, [A]ll, [N]one, [r]ename: y
 extracting: home/ctf-player/drop-in/flag.png  
```

3. Nos movemos a la carpeta descomprimida
```
cd home

ls
ctf-player

cd ctf-player

ls
drop-in

cd drop-in   

ls
flag.png

open flag.png
```

4. La imagen en  un código QR. Podemos escanearlo directamente con el celular o usar un comando. Obtenemos la bandera
```
zbarimg flag.png

QR-Code:picoCTF{p33k_@_b00_3f7cf1ae}
scanned 1 barcode symbols from 1 images in 0.01 seconds
```



# CanYouSee

How about some hide and seek? Download this file [here](https://artifacts.picoctf.net/c_titan/128/unknown.zip).

How can you view the information about the picture?
If something isn't in the expected form, maybe it deserves attention?

1. Descargamos el archivo
```
wget https://artifacts.picoctf.net/c_titan/128/unknown.zip
```

2. Descomprimimos
```
unzip unknown.zip   

Archive:  unknown.zip
  inflating: ukn_reality.jpg  
```

3. Revisamos los metadatos
```
exiftool ukn_reality.jpg

ExifTool Version Number         : 13.00
File Name                       : ukn_reality.jpg
Directory                       : .
File Size                       : 2.3 MB
File Modification Date/Time     : 2024:03:11 18:05:51-06:00
File Access Date/Time           : 2025:05:11 19:34:42-06:00
File Inode Change Date/Time     : 2025:05:11 19:34:42-06:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 72
Y Resolution                    : 72
XMP Toolkit                     : Image::ExifTool 11.88
Attribution URL                 : cGljb0NURntNRTc0RDQ3QV9ISUREM05fM2I5MjA5YTJ9Cg==
Image Width                     : 4308
Image Height                    : 2875
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 4308x2875
Megapixels                      : 12.4
```

4. Attribution URL parece estar encripatado en base 64
```
echo "cGljb0NURntNRTc0RDQ3QV9ISUREM05fM2I5MjA5YTJ9Cg==" | base64 -d
picoCTF{ME74D47A_HIDD3N_3b9209a2}
```



# Secret of the Polyglot

The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file? Download the suspicious file [here](https://artifacts.picoctf.net/c_titan/97/flag2of2-final.pdf).

This problem can be solved by just opening the file in different ways

1. Descargamos el archivo
```
wget https://artifacts.picoctf.net/c_titan/97/flag2of2-final.pdf
```

2. Revisamos el archivo. Resulta que es un png
```
file flag2of2-final.pdf

flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced
```

3. Revisamos el primero bytes
```
xxd flag2of2-final.pdf | head -n 25
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 0032 0000 0032 0806 0000 001e 3f88  ...2...2......?.
00000020: b100 0001 8569 4343 5049 4343 2070 726f  .....iCCPICC pro
00000030: 6669 6c65 0000 2891 7d91 3d48 c340 1cc5  file..(.}.=H.@..
00000040: 5f53 a5a2 1511 3b88 0866 a80e 6241 54c4  _S....;..f..bAT.
00000050: 51ab 5084 0aa1 5668 d5c1 e4d2 2f68 d290  Q.P...Vh..../h..
00000060: a4b8 380a ae05 073f 16ab 0e2e ceba 3ab8  ..8....?......:.
00000070: 0a82 e007 88ab 8b93 a28b 94f8 bfa4 d022  ..............."
00000080: d683 e37e bcbb f7b8 7b07 08d5 22d3 acb6  ...~....{..."...
00000090: 7140 d36d 3311 8b8a a9f4 aa18 7845 1704  q@.m3.......xE..
000000a0: f462 1443 32b3 8c39 498a a3e5 f8ba 878f  .b.C2..9I.......
000000b0: af77 119e d5fa dc9f a35b cd58 0cf0 89c4  .w.......[.X....
000000c0: b3cc 306d e20d e2e9 4ddb e0bc 4f1c 6279  ..0m....M...O.by
000000d0: 5925 3e27 1e33 e982 c48f 5c57 3c7e e39c  Y%>'.3....\W<~..
000000e0: 7359 e099 2133 9998 270e 118b b926 569a  sY..!3..'....&V.
000000f0: 98e5 4d8d 788a 38ac 6a3a e50b 298f 55ce  ..M.x.8.j:..).U.
00000100: 5b9c b562 99d5 efc9 5f18 cce8 2bcb 5ca7  [..b...._...+.\.
00000110: 3988 1816 b104 0922 1494 5140 1136 22b4  9......"..Q@.6".
00000120: eaa4 5848 d07e b485 7fc0 f54b e452 c855  ..XH.~.....K.R.U
00000130: 0023 c702 4ad0 20bb 7ef0 3ff8 ddad 959d  .#..J. .~.?.....
00000140: 9cf0 9282 51a0 fdc5 713e 8681 c02e 50ab  ....Q...q>....P.
00000150: 38ce f7b1 e3d4 4e00 ff33 70a5 37fc a52a  8.....N..3p.7..*
00000160: 30f3 497a a5a1 858f 809e 6de0 e2ba a129  0.Iz......m....)
00000170: 7bc0 e50e d0ff 64c8 a6ec 4a7e 9a42 360b  {.....d...J~.B6.
00000180: bc9f d137 a581 be5b a073 cdeb adbe 8fd3  ...7...[.s......
```
Tiene la extensión pdf, pero igual se abrirá como png

4. Cambiamos la extensión a png para poder abrirlo
```
mv flag2of2-final.pdf flag2of2-final.png
```

5. Abrimos la imagen y obtenemos la primera parte de la bandera
```
picoCTF{f1u3n7_
```

6. Regresamos el archivo a la extensión pdf
```
mv flag2of2-final.png flag2of2-final.pdf
```

7. Obtenemos la segunda parte de la bandera
```
1n_pn9_&_pdf_724b1287}
```

8. Unificamos ambas partes
```
picoCTF{f1u3n7_1n_pn9_&_pdf_724b1287}
```



# RED

RED, RED, RED, RED Download the image: [red.png](https://challenge-files.picoctf.net/c_verbal_sleep/831307718b34193b288dde31e557484876fb84978b5818e2627e453a54aa9ba6/red.png)

The picture seems pure, but is it though?
Red?Ged?Bed?Aed?
Check whatever Facebook is called now.

1. Descargamos el archivo
```
wget https://challenge-files.picoctf.net/c_verbal_sleep/831307718b34193b288dde31e557484876fb84978b5818e2627e453a54aa9ba6/red.png
```

2. Revisamos los metadatos
```
exiftool red.png

ExifTool Version Number         : 13.00
File Name                       : red.png
Directory                       : .
File Size                       : 796 bytes
File Modification Date/Time     : 2025:03:05 21:34:15-06:00
File Access Date/Time           : 2025:05:11 20:04:29-06:00
File Inode Change Date/Time     : 2025:05:11 20:04:29-06:00
File Permissions                : -rw-rw-r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 128
Image Height                    : 128
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Poem                            : Crimson heart, vibrant and bold,.Hearts flutter at your sight..Evenings glow softly red,.Cherries burst with sweet life..Kisses linger with your warmth..Love deep as merlot..Scarlet leaves falling softly,.Bold in every stroke.
Image Size                      : 128x128
Megapixels                      : 0.016
```

3. Aparentemente no hay nada de raro pero si nos fijamos bien, las letras mayúsculas del poema forman la palabra CHECK LSB, así que extraemos los datos ocultos en bits menos significativos
```
zsteg --lsb red.png

meta Poem           .. text: "Crimson heart, vibrant and bold,\nHearts flutter at your sight.\nEvenings glow softly red,\nCherries burst with sweet life.\nKisses linger with your warmth.\nLove deep as merlot.\nScarlet leaves falling softly,\nBold in every stroke."                                                                
b1,rgba,lsb,xy      .. text: "cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ=="                                         
b2,g,lsb,xy         .. text: "ET@UETPETUUT@TUUTD@PDUDDDPE"
b2,rgb,lsb,xy       .. file: OpenPGP Secret Key
b2,rgba,lsb,xy      .. file: OpenPGP Secret Key
b2,abgr,lsb,xy      .. file: OpenPGP Secret Key
b4,b,lsb,xy         .. file: 0421 Alliant compact executable not stripped
```

4. Desencriptamos desde base 64
```
echo "cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==" | base64 -d

picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}     
```
La bandera no puede tener más de 128 caractereres

5. Probamos con la primera bandera y sí funciona
```
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```



# Ph4nt0m 1ntrud3r

A digital ghost has breached my defenses, and my sensitive data has been stolen! 😱💻 Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag. To solve this challenge, you'll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner. Dive into the network traffic, apply the right filters and show off your forensic prowess and unmask the digital intruder! Find the PCAP file here [Network Traffic PCAP file](https://challenge-files.picoctf.net/c_verbal_sleep/960abba2fdbc9be5013ef87f1df67213e9b63d4561d7a8c8c1ce7a4ce40a547e/myNetworkTraffic.pcap) and try to get the flag.

Filter your packets to narrow down your search.
Attacks were done in timely manner.
Time is essential

1. Descargamos el archivo
```
wget https://challenge-

files.picoctf.net/c_verbal_sleep/960abba2fdbc9be5013ef87f1df67213e
9b63d4561d7a8c8c1ce7a4ce40a547e/myNetworkTraffic.pcap
```

2. Abrimos el archivo en WireShark
```
wireshark myNetworkTraffic.pcap   
```
Al iniciar, muestra la advertencia que WireShark está tratando de cargar una clave TLS, pero no tenemos este archivo o no tenemos los permisos. Parece que no hay mucho que podamos hacer aquí.

3. Filtramos los datos contenidos en los segmentos TCP que tienen exactamente 12 bytes de longitud y los convertimos desde base 64
```
tshark -r myNetworkTraffic.pcap -Y "tcp.len==12" -T fields -e tcp.segment_data | xxd -p -r | base64 -d

tshark: Error loading table 'TLS Decrypt': ssl_keys:2: File '/home/sailork/picopico.key' does not exist or access is denied.
{1t_w4spicoCTF_34sy_tbh_4r_e5e8c78dnt_th4t     
```
Parece que encontramos la bandera pero está desordenada.

4. Hacemos el paso anterior, pero ahora ordenamos por paquetes por tiempo
```
tshark -r myNetworkTraffic.pcap -Y "tcp.len==12 || tcp.len==4" -T fields -e frame.time -e tcp.segment_data | sort -k4 | awk '{print $6}' | xxd -p -r | base64 -d

tshark: Error loading table 'TLS Decrypt': ssl_keys:2: File '/home/sailork/picopico.key' does not exist or access is denied.
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_e5e8c78d}    
```



# flags are stepic

A group of underground hackers might be using this legit site to communicate. Use your forensic techniques to uncover their message Try it [here](http://standard-pizzas.picoctf.net:54332/)!

In the country that doesn't exist, the flag persists

1. Visitamos el sitio web
```
http://standard-pizzas.picoctf.net:54332/
```
Buscamos la imagen de una bandera de un país que no existe. Encontramos uno llamado "Upanzi, Republic The". Inspeccionamos para encontrar el nombre de la imagen

2. Descargamos la imagen de la bandera
```
wget http://standard-pizzas.picoctf.net:54332/flags/upz.png
```

3. Usamos una herramienta de esteganografía para decodificar la bandera
```
stepic -i upz.png -d

/home/sailork/.local/share/pipx/venvs/stepic/lib/python3.12/site-packages/PIL/Image.py:3442: DecompressionBombWarning: Image size (150658990 pixels) exceeds limit of 89478485 pixels, could be decompression bomb DOS attack.
  warnings.warn(
picoCTF{fl4g_h45_fl4ga664459a}    
```