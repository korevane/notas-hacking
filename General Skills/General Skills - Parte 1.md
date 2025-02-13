# 2Warm

Convertir el numero 24 (base 10) a binario (base 2)

```
>>> bin(42)
'0b101010'
>>> bin(42)[2:]
'101010'

picoCTF{101010}
```

Se debe acceder a la cadena desde la posición 2 para quitar el '0b'.


# Bases

What does this `bDNhcm5fdGgzX3IwcDM1` mean? I think it has something to do with bases.

**En CyberChef**
```
input: bDNhcm5fdGgzX3IwcDM1
recipe: from base 64
output: l3arn_th3_r0p35
```

**En LINUX**
```
input: bDNhcm5fdGgzX3IwcDM1
echo bDNhcm5fdGgzX3IwcDM1 | base64 -d
l3arn_th3_r0p35
```

**En Python**
```
En Python
import base64
base64.b64decode('bDNhcm5fdGgzX3IwcDM1')
b'l3arn_th3_r0p35'
```

```
picoCTF{l3arn_th3_r0p35}
```


# First Grep

Can you find the flag in file? This would be really tedious to look through manually, something tells me there is a better way.

Usamos el comando grep para buscar la palabra clave pico y encontrar la bandera
```
wget LINK_DEL_ARCHIVO
cat file  | grep pico

picoCTF{grep_is_good_to_find_things_f77e0797}
```

https://ryanstutorials.net/linuxtutorial/grep.php

# Glitch Cat

Our flag printing service has started glitching!
Additional details will be available after launching your challenge instance.

This challenge launches an instance on demand.  Its current status is: `NOT_RUNNING`
ASCII is one of the most common encodings used in programming
We know that the glitch output is valid Python, somehow!
Press Ctrl and c on your keyboard to close your connection and return to the command prompt.

1. Nos conectamos al puerto
	```
	nc saturn.picoctf.net 57292
	```

2. Como resultado obtenemos un código de python
	```
	'picoCTF{gl17ch_m3_n07_' + chr(0x39) + chr(0x63) + chr(0x34) + chr(0x32) + chr(0x61) + chr(0x34) + chr(0x35) + chr(0x64) + '}'
	```

3. Lo ejecutamos en python y obtenemos la llave
```
picoCTF{gl17ch_m3_n07_9c42a45d}
```

# what's a net cat?

Using netcat (nc) is going to be pretty important. Can you connect to `jupiter.challenges.picoctf.org` at port `41120` to get the flag?

```
nc jupiter.challenges.picoctf.org 41120
You're on your way to becoming the net cat master
picoCTF{nEtCat_Mast3ry_3214be47}
```

netcad es para saber si hay puertos de conexión abiertos.

https://linux.die.net/man/1/nc

# plumbing

Sometimes you need to handle process data outside of a file. Can you find a way to keep the output from this program and search for the flag? Connect to `jupiter.challenges.picoctf.org 4427`.

**Guardando la salida en un archivo**
```
nc jupiter.challenges.picoctf.org 4427 > salida
cat salida | grep pico
picoCTF{digital_plumb3r_5ea1fbd7}
```

**Buscando la llave directamente**
```
nc jupiter.challenges.picoctf.org 4427 | grep pico
picoCTF{digital_plumb3r_5ea1fbd7}
picoCTF{digital_plumb3r_5ea1fbd7}
```

https://www.linfo.org/pipes.html

# Nice netcat...

There is a nice program that you can talk to by using this command in a shell: `$ nc mercury.picoctf.net 43239`, but it doesn't speak English...

1. Nos conectamos al puerto
	```
	nc mercury.picoctf.net 43239
	```

2. Obtenemos una salida con valores decimales
	```
	112 
	105 
	99 
	...
	```

3. Usamos CyberChef para convertirlos de decimal a ASCII
```
input: 112 105 99...
recipe: from decimal
output: picoCTF{g00d_k1tty!_n1c3_k1tty!_7c0821f5}
```
# Obedient Cat

This file has a flag in plain sight (aka "in-the-clear").
Any hints about entering a command into the Terminal (such as the next one), will start with a '$'... everything after the dollar sign will be typed (or copy and pasted) into your Terminal.
To get the file accessible in your shell, enter the following in the Terminal prompt: `$ wget https://mercury.picoctf.net/static/217686fc11d733b80be62dcfcfca6c75/flag`
``$ man cat``

1. Se descarga el archivo
	```
	 wget https://mercury.picoctf.net/static/217686fc11d733b80be62dcfcfca6c75/flag
	```

2. Se usa grep
	```
	cat flag.1 | grep pico
	```

3. Se obtiene la bandera
	```
	picoCTF{s4n1ty_v3r1f13d_b5aeb3dd}
	```


# Warmed Up

What is 0x3D (base 16) in decimal (base 10)?

**En Pyhton**
```
int(0x3D)
picoCTF{61}

int('3D',16)
picoCTF{61}
```

**En CyberChef**
```
input: 3D
recipe: From Hex + To Decimal
output: 61
```


# Lets Warm Up

If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII?

```
bytes.fromhex('70').decode('ascii')
picoCTF{'p'}
```

Además de 'ascii' también se puede usar:
- `'utf-8'`: Decodifica usando la codificación UTF-8.
- `'latin-1'`: Decodifica usando la codificación Latin-1 (ISO-8859-1).
- `'utf-16'`: Decodifica usando la codificación UTF-16.
- `'cp1252'`: Decodifica usando la codificación Windows-1252.

