
# Glory of the Garden

This [garden](https://jupiter.challenges.picoctf.org/static/4153422e18d40363e7ffc7e15a108683/garden.jpg) contains more than it seems.
What is a hex editor?

1. Descargamos la imagen y buscamos qué tipo de archivo es
```
file garden.jpg   

output:
garden.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 72x72, segment length 16, baseline, precision 8, 2999x2249, components 3
```

```
open garden.jpg

output: visualizamos la imagen
```

2. Abrimos la imagen en el editor hexadecimal
```
hexeditor garden.jpg 
```

3. Usamos ctrl + w para buscar la bandera
```
00230560  72 65 20 69  73 20 61 20   66 6C 61 67  20 22 70 69      re is a flag "pi
00230570  63 6F 43 54  46 7B 6D 6F   72 65 5F 74  68 61 6E 5F      coCTF{more_than_
00230580  6D 33 33 74  73 5F 74 68   65 5F 33 79  33 33 64 64      m33ts_the_3y33dd
00230590  32 65 45 46  35 7D 22 0A                                 2eEF5}".     
```

```
picoCTF{more_than_m33ts_the_3y33dd2eEF5}
```


4. Otra opción es usar el comando strings para mostrar todas las cadenas que hay en hexadecimal y restringir la búsqueda a cadenas con más de n cantidad de caracteres para encontrar la bandera
```
strings -n 40 garden.jpg

output:
Copyright (c) 1998 Hewlett-Packard Company
.IEC 61966-2.1 Default RGB colour space - sRGB
.IEC 61966-2.1 Default RGB colour space - sRGB
,Reference Viewing Condition in IEC61966-2.1
,Reference Viewing Condition in IEC61966-2.1
%&'()*456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
&'()*56789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
Here is a flag "picoCTF{more_than_m33ts_the_3y33dd2eEF5}"
```

O mejor aún, buscar coincidencias directamente
```
strings -n 9 garden.jpg | grep picoCTF

output:
Here is a flag "picoCTF{more_than_m33ts_the_3y33dd2eEF5}"
```

**Referencias:**
- [Video referencia](https://www.youtube.com/watch?v=xxhnGxgOtWs)
- [hexeditor](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbFVJRHJnbEtpUTNEdjNSSk9ISE54eVNuQUNWQXxBQ3Jtc0tuay0zNlZvRElVd2lzZ3NwRm5BUDN0RWNyZFg0Nzc4Tzd0SFhhQWQ1M3NYdUUwbWpkTjlQXzh0VU1wRllxcjVYTENROGJrbEJGOG54STJjVFhGTms1SkpNRnYybXlGdnhDak5vRTFkMTVTb3ZBSzJzZw&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FHex_editor&v=xxhnGxgOtWs)


# So Meta

Find the flag in this [picture](https://jupiter.challenges.picoctf.org/static/00efdf2961da1e21470ffc0d496c3cc2/pico_img.png).
What does meta mean in the context of files?
Ever heard of metadata?

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/00efdf2961da1e21470ffc0d496c3cc2/pico_img.png
```

2. Examinamos los meta datos
```
exiftool pico_img.png

output:
ExifTool Version Number         : 13.00
File Name                       : pico_img.png
Directory                       : .
File Size                       : 109 kB
File Modification Date/Time     : 2020:10:26 12:38:23-06:00
File Access Date/Time           : 2025:04:05 11:09:14-06:00
File Inode Change Date/Time     : 2025:04:05 11:09:14-06:00
File Permissions                : -rw-rw-r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 600
Image Height                    : 600
Bit Depth                       : 8
Color Type                      : RGB
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Software                        : Adobe ImageReady
XMP Toolkit                     : Adobe XMP Core 5.3-c011 66.145661, 2012/02/06-14:56:27
Creator Tool                    : Adobe Photoshop CS6 (Windows)
Instance ID                     : xmp.iid:A5566E73B2B811E8BC7F9A4303DF1F9B
Document ID                     : xmp.did:A5566E74B2B811E8BC7F9A4303DF1F9B
Derived From Instance ID        : xmp.iid:A5566E71B2B811E8BC7F9A4303DF1F9B
Derived From Document ID        : xmp.did:A5566E72B2B811E8BC7F9A4303DF1F9B
Warning                         : [minor] Text/EXIF chunk(s) found after PNG IDAT (may be ignored by some readers)
Artist                          : picoCTF{s0_m3ta_fec06741}
Image Size                      : 600x600
Megapixels                      : 0.360
```

```
exiftool pico_img.png -Artist

output:
Artist                          : picoCTF{s0_m3ta_fec06741}
```

```
strings -n 9 pico_img.png | grep picoCTF 

output:
picoCTF{s0_m3ta_fec06741}
```

**Referencias:**
- [Video](https://www.youtube.com/watch?v=Govu_p-wf4I)
- [metadata](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbG1IREw0aFJjeVBEOGU2a05ZbTdabWFjc3RDd3xBQ3Jtc0trdFBpUnlWeTlYMzNqb05odDFMT0llb3hxd21xaDFSTkpmRWg1SmY4UmNPR1c1LU9sQlVxTThINWJ0Yk5tSUFXNDZiWGxmMjhOamM1cm5SWjUwb3RUdXg5MFZlTFFCN0ZaYXRBNjdOMUF5eVB6d2NOZw&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMetadata&v=Govu_p-wf4I)


# shark on wire 1

We found this [packet capture](https://jupiter.challenges.picoctf.org/static/483e50268fe7e015c49caf51a69063d0/capture.pcap). Recover the flag.
Try using a tool like Wireshark
What are streams?

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/483e50268fe7e015c49caf51a69063d0/capture.pcap
```

2. Revisamos qué tipo de archivo es
```
file capture.pcap

output:
capture.pcap: pcap capture file, microsecond ts (little-endian) - version 2.4 (Ethernet, capture length 262144)
```

3. Abrimos el archivo en wireshark

4. Buscamos el primer paquete UDP. Click derecho. Seguir. UDP Stream. Avanzamos entre los streams o grupos de paquetes hasta encontrar la bandera. En este caso, encontramos la bandera en el stream 6
```
picoCTF{StaT31355_636f6e6e}
```


**Referencias:**
- [Video](https://www.youtube.com/watch?v=q8cM4sY0izw)
- [pcap](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3BKSGhYalYxWWVVQURweUZrdmplbU16aXJjQXxBQ3Jtc0tuWUJva3UwRDBxckZuaFQ5cTE4MjhlUTZxUFY1VjQ5eGczZDRBT0tMc3BJRzFDdnBaYnlMREZoano5QmROZmY5MU5QUUhMa1VYZmFUS2llSFZqc0xWRDlqM2tHYmVoY1ZpMHdxNDRkMzN1N2tNcXRmNA&q=https%3A%2F%2Fwww.comparitech.com%2Fnet-admin%2Fpcap-guide%2F&v=q8cM4sY0izw)


# extensions

This is a really weird text file [TXT](https://jupiter.challenges.picoctf.org/static/e7e5d188621ee705ceeb0452525412ef/flag.txt)? Can you find the flag?
How do operating systems know what kind of file it is? (It's not just the ending!
Make sure to submit the flag as picoCTF{XXXXX}

1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/e7e5d188621ee705ceeb0452525412ef/flag.txt 
```

2. Revisamos el archivo. Es un archivo png con extensión txt
```
file flag.txt 

output:
flag.txt: PNG image data, 1697 x 608, 8-bit/color RGB, non-interlaced
```

3. Revisamos la firma de los archivos png
```
xxd flag.txt | head 

output:
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 06a1 0000 0260 0802 0000 0085 ad5e  .......`.......^
00000020: 9a00 0000 0173 5247 4200 aece 1ce9 0000  .....sRGB.......
00000030: 0004 6741 4d41 0000 b18f 0bfc 6105 0000  ..gAMA......a...
00000040: 0009 7048 5973 0000 1625 0000 1625 0149  ..pHYs...%...%.I
00000050: 5224 f000 0026 9549 4441 5478 5eed dd6b  R$...&.IDATx^..k
00000060: 421b 39b7 05d0 3b2e 0694 f130 9a4c 2683  B.9...;....0.L&.
00000070: f9ae 5f80 4e3d 25bb 4cb3 f15a bfba a14a  .._.N=%.L..Z...J
00000080: 7574 2413 7927 c0ff fd0f 0000 0000 4826  ut$.y'........H&
00000090: e303 0000 0080 6c32 3e00 0000 00c8 26e3  ......l2>.....&.
```

4. Cambiamos a la extensión de txt a png y abrimos el archivo
```
mv flag.txt flag.png  
open flag.png
```

5. Encontramos la bandera
```
picoCTF{now_you_know_about_extensions}
```


**Referencias:**
- [Video](https://www.youtube.com/watch?v=FbFpIS60M_s)
- [file format](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbEFiaGZicDlKaDZPUjcxUkxLU3lTdGcwbW1OUXxBQ3Jtc0ttYWR5NHNic0JDLTZNU0JJOExjTHFXcV9rX1h4N1FzQl9wa2R4b3I5NGEtZEVIOXFqVl9xT2pFX3VOZlhpSEFvMmZQaWUyeDZqa012R2tPcGZ4a2hycUdBYjJQT0NIY3VURkk4SkhTZS1yMGgwODREMA&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FFile_format&v=FbFpIS60M_s)
- [file signatures](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0k0aU5XVDE1RUZtNW1CSmU0SXJnejBGTFNkUXxBQ3Jtc0ttQVA0X1FtQnVQdWZYMVk2RjBxdUtFb295WUxoVGdiUDVtMFJwYXp4LVRkd2dQTEZQWEttUTdHaE51UUFGWTk3R0xsZXh3cU91TDFiMW9hYUVTbHVmUnpTWS1lVkk0SkMxNHJGR21CYVZsbGI2V1BpVQ&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FList_of_file_signatures%2F&v=FbFpIS60M_s)

# What Lies Within

There's something in the [building](https://jupiter.challenges.picoctf.org/static/011955b303f293d60c8116e6a4c5c84f/buildings.png). Can you retrieve the flag?
There is data encoded somewhere... there might be an online decoder.


1. Descargamos el archivo
```
wget https://jupiter.challenges.picoctf.org/static/011955b303f293d60c8116e6a4c5c84f/buildings.png
```

2. Revisamos el tipo de archivo
```
file buildings.png

output:
buildings.png: PNG image data, 657 x 438, 8-bit/color RGBA, non-interlaced
```

3. Abrimos el archivo
```
open buildings.png
```

4. Hay información oculta en la imagen, así que usamos steganography online para subir la imagen y decodificar lo que sea que haya dentro y obtener la bandera.
```
picoCTF{h1d1ng_1n_th3_b1t5}
```

También se puede usar zsteg
```
zsteg -a buildings.png | grep picoCTF

output:
b1,rgb,lsb,xy       .. text: "picoCTF{h1d1ng_1n_th3_b1t5}"
```

**Referencias:**
- [Video](https://www.youtube.com/watch?v=bFUB-USG3sw)
- [steganography](https://www.simplilearn.com/what-is-steganography-article)
- [steganography online](https://stylesuxx.github.io/steganography/)