# Permissions

Can you read files in the root file?The system admin has provisioned an account for you on the main server:`ssh -p 50841 picoplayer@saturn.picoctf.net`Password: `UYiOazkqY2`Can you login and read the root file?

1. Iniciamos sesión con ssh
	```
	picoCTF{uS1ng_v1m_3dit0r_89e9cf1a}
	password: UYiOazkqY2
	```

2. Ejecutamos el comando ``sudo -l``
	```
	sudo -l
	output: 
	Matching Defaults entries for picoplayer on challenge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

	User picoplayer may run the following commands on challenge:
    (ALL) /usr/bin/vi
	```
	Este comando lista los privilegios que tenemos como usuario. Saber qué comandos podemos ejecutar con permisos de superusuario (root) permiten planificar una estrategia. En este caso vemos que podemos usar ``usr, bin y vi``.

3. Accedemos al editor de texto Vim.
	```
	sudo vi ó sudo vim
	```
	Vim es un editor de texto con la capacidad de ejecutar comandos del sistema si se abre con permisos elevados (sudo). Usamos Vim para editar archivos y acceder a un shell de root.

4. Ejecutamos un shell dentro de Vim.
	```
	:!sh
	```
	``:!sh`` ejecuta un shell (sh) desde dentro de Vim con los mismos permisos que se usaron para abrir Vim. Como abrimos Vim con sudo, este shell tendrá permisos de superusuario, permitiéndonos ejecutar comandos con los privilegios de root.

5. Navegar al directorio raíz
	```
	cd /root
	```
	``/root`` es el directorio home del usuario root. Es común que los archivos que contienen la bandera en desafíos CTF se encuentren en ubicaciones accesibles sólo para el usuario root.

6. Encontrar y leer el archivo de la bandera
	```
	ls -a
	cat .flag.txt
	```
	``ls -a`` muestra todos los archivos, incluidos los ocultos (los que comienzan con un punto). En muchos desafíos, las banderas se ocultan para añadir dificultad. ``cat .flag.txt`` muestra el contenido del archivo, revelando la bandera.


# chrono

How to automate tasks to run at intervals on linux servers? Use ssh to connect to this server:
Server: saturn.picoctf.net
Port: 58086
Username: picoplayer 
Password: 5wf1w1hVxt

1. Conectar por ssh
	```
	ssh -p 58086 picoplayer@saturn.picoctf.net
	password: 5wf1w1hVxt
	```

2. Verificar los cron jobs del sistema y encontrar la bandera
	```
	cat /etc/crontab | grep picoCTF
	```
	Los cron jobs son tareas programadas que se ejecutan automáticamente en intervalos específicos. Para ver los cron jobs del sistema de debe acceder al archivo ``/etc/crontab``. Buscamos e imprimimos la bandera con ``cat`` y ``grep``.


# Special

Don't power users get tired of making spelling mistakes in the shell? Not anymore! Enter Special, the Spell Checked Interface for Affecting Linux. Now, every word is properly spelled and capitalized... automatically and behind-the-scenes! Be the first to test Special in beta, and feel free to tell us all about how Special streamlines every development process that you face. When your co-workers see your amazing shell interface, just tell them: That's Special (TM)Start your instance to see connection details.`ssh -p 53086 ctf-player@saturn.picoctf.net`The password is `d8819d45`

Experiment with different shell syntax. 

1. Nos conectamos con ssh
	```
	ssh -p 53086 ctf-player@saturn.picoctf.net
	password: d8819d45
	```

2. Notamos que hay una diferencia en este shell; no podemos ejecutar comandos básicos como ``ls, cat o pwd``. 
	```
	Special$ ls
	Is 
	sh: 1: Is: not found
	Special$ cat
	Cat 
	sh: 1: Cat: not found
	Special$ pwd
	Pod 
	sh: 1: Pod: not found
	```
	La interfaz está validando y modificando los comandos.

3. Evitar la validación.
	```
	""""""ls
	output: 
		""""ls 
		blargh
	```
	Usamos comillas dobles ``"`` al inicio del comando. También es posible usar comillas simples ``''''''ls``, usar secuencias de escape del shell ``\ls``, concatenación de comandos ``echo $(ls)``, usar variables de entorno ``$(echo ls)``, usar un shell alternativo como ``/bin/sh`` en lugar de ``/bin/bash``, por ejemplo ``sh -c 'ls'``.

4. Encontrar y leer el archivo de la bandera.
	```
	""""""cat blargh/flag.txt
	output: picoCTF{5p311ch3ck_15_7h3_w0r57_0c61d335}Special$
	```


# Commitment Issues

I accidentally wrote the flag down. Good thing I deleted it! You download the challenge files here: challenge.zip. 

Version control can help you recover files if you change or lose them! Read the chapter on Git from the picoPrimer [here](https://primer.picoctf.org/#_git_version_control). You can 'checkout' commits to see the files inside them.

1. Descargamos el archivo.zip
	```
	wget LINK-ARCHIVO.zip
	```

2. Descomprimimos el archivo.
	```
	unzip ARCHIVO.zip
	```

3. Vemos que el directorio principal se llama drop-in y contiene un repositorio Git.
	```
	cd drop-in
	```
	El repositorio Git contiene la historia de los commits.

4. Explotamos el historial de Git.
	```
	git log
	```
	Se muestra una lista de los commits realizados en el repositorio.

5. Buscamos el commit con la bandera y anotamos el ID del commit.
	```
	commit 87b85d7dfb839b077678611280fa023d76e017b8 (HEAD)
	Author: picoCTF <ops@picoctf.com>
	Date:   Tue Mar 12 00:06:17 2024 +0000
	
	    create flag
	```

6. Cambiamos al commit de la bandera.
	```
	git checkout 87b85d7dfb839b077678611280fa023d76e017b8
	```
	Así podremos ver el estado del repositorio en ese momento específico.

7. Revisamos los archivos que había en ese momento.
	```
	ls
	output: message.txt
	```

8. Leemos el archivo.
	```
	cat message.txt
	ouput: picoCTF{s@n1t1z3_ea83ff2a}
	```


# Time Machine

What was I last working on? I remember writing a note to help me remember...
You can download the challenge files here: challenge.zip
The `cat` command will let you read a file, but that won't help you here!
Read the chapter on Git from the picoPrimer [here](https://primer.picoctf.org/#_git_version_control).
When committing a file with git, a message can (and should) be included.

1. Descargamos el archivo.zip
	```
	wget LINK-ARCHIVO.zip
	```

2. Descomprimimos el archivo.
	```
	unzip ARCHIVO.zip
	```

3. Vemos que el directorio principal se llama drop-in y contiene un repositorio Git.
	```
	cd drop-in
	```
	El repositorio Git contiene la historia de los commits.

4. Explotamos el historial de Git.
	```
	git log
	```
	Se muestra una lista de los commits realizados en el repositorio.

5. Buscamos el commit con la bandera.
	```
	commit 89d296ef533525a1378529be66b22d6a2c01e530 (HEAD -> master)
	Author: picoCTF <ops@picoctf.com>
	Date:   Tue Mar 12 00:07:22 2024 +0000
	
		picoCTF{t1m3m@ch1n3_186cd7d7}
	```


# Blame Game

Someone's commits seems to be preventing the program from working. Who is it? You can download the challenge files here: challenge.zip

In collaborative projects, many users can make many changes. How can you see the changes within one file?
Read the chapter on Git from the picoPrimer [here](https://primer.picoctf.org/#_git_version_control).
You can use `python3 <file>.py` to try running the code, though you won't need to for this challenge.

1. Descargamos el archivo.zip
	```
	wget LINK-ARCHIVO.zip
	```

2. Descomprimimos el archivo.
	```
	unzip ARCHIVO.zip
	```

3. Vemos que el directorio principal se llama drop-in y contiene un repositorio Git.
	```
	cd drop-in
	```
	El repositorio Git contiene la historia de los commits.

4. Explotamos el historial de Git.
	```
	git log
	```
	Se muestra una lista de los commits realizados en el repositorio.

5. Buscamos el commit con la bandera.
	```
	commit 0fe87f16cbd8129ed5f7cf2f6a06af6688665728
	Author: picoCTF{@sk_th3_1nt3rn_ea346835} <ops@picoctf.com>
	Date:   Sat Mar 9 21:09:25 2024 +0000
	
	    optimize file size of prod code
	```
	Encontramos la bandera escondida en Author.


# Collaborative Development

My team has been working very hard on new features for our flag printing program! I wonder how they'll work together?
You can download the challenge files here: challenge.zip

`git branch -a` will let you see available branches
How can file 'diffs' be brought to the main branch? Don't forget to `git config`!
Merge conflicts can be tricky! Try a text editor like nano, emacs, or vim.

1. Descargar el archivo 
	```
	wget LINK-ARCHIVO.zip
	```

2. Descomprimir el archivo
	```
	unzip ARCHIVO.zip
	```

3. Nos dirigimos al directorio que contiene un repositorio Git.
	```
	cd drop-in
	```

4. Revisamos la lista de todas las ramas disponibles.
	```
	git branch -a
	```

5. Buscamos en cada rama.
	```
	SailorK3-picoctf@webshell:~/drop-in$ git checkout feature/part-1
	Switched to branch 'feature/part-1'
	SailorK3-picoctf@webshell:~/drop-in$ cat flag.py
	print("Printing the flag...")
	print("picoCTF{t3@mw0rk_", end='')
	
	SailorK3-picoctf@webshell:~/drop-in$ git checkout feature/part-2
	Switched to branch 'feature/part-2'
	SailorK3-picoctf@webshell:~/drop-in$ cat flag.py
	print("Printing the flag...")
	print("m@k3s_th3_dr3@m_", end='')
	
	SailorK3-picoctf@webshell:~/drop-in$ git checkout feature/part-3
	Switched to branch 'feature/part-3'
	SailorK3-picoctf@webshell:~/drop-in$ cat flag.py
	print("Printing the flag...")
	print("w0rk_7ae8dd33}")
	```

6. Unificamos la bandera fraccionada.
	```
	picoCTF{t3@mw0rk_m@k3s_th3_dr3@m_w0rk_7ae8dd33}
	```


# Binary Search

Want to play a game? As you use more of the shell, you might be interested in how they work! Binary search is a classic algorithm used to quickly find an item in a sorted list. Can you find the flag? You'll have 1000 possibilities and only 10 guesses.

Cyber security often has a huge amount of data to look through - from logs, vulnerability reports, and forensics. Practicing the fundamentals manually might help you in the future when you have to write your own tools!
You can download the challenge files here: challenge.zip

`ssh -p 50561 ctf-player@atlas.picoctf.net`Using the password `6dd28e9b`. Accept the fingerprint with `yes`, and `ls` once connected to begin. Remember, in a shell, passwords are hidden!

Have you ever played hot or cold? Binary search is a bit like that.
You have a very limited number of guesses. Try larger jumps between numbers!
The program will randomly choose a new number each time you connect. You can always try again, but you should start your binary search over from the beginning - try around 500. Can you think of why?

1. Conectarse al servidor por ssh
	```
	ssh -p 50561 ctf-player@atlas.picoctf.net
	password: 6dd28e9b
	```

2. Ganar el juego para obtener la bandera
	```
	Welcome to the Binary Search Game!
	I'm thinking of a number between 1 and 1000.
	Enter your guess: 500
	Lower! Try again.
	Enter your guess: 250
	Higher! Try again.
	Enter your guess: 375
	Lower! Try again.
	Enter your guess: 312
	Lower! Try again.
	Enter your guess: 281
	Lower! Try again.
	Enter your guess: 265
	Higher! Try again.
	Enter your guess: 273
	Lower! Try again.
	Enter your guess: 269
	Lower! Try again.
	Enter your guess: 267
	Higher! Try again.
	Enter your guess: 268
	Congratulations! You guessed the correct number: 268
	Here's your flag: picoCTF{g00d_gu355_de9570b0}
	Connection to atlas.picoctf.net closed.
	```

Se intentó usar un código en Python para automatizar el proceso pero no fue posible implementarlo. 

```python
import paramiko

def binary_search_ssh(host, port, username, password):
    client = paramiko.SSHClient()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    client.connect(host, port=port, username=username, password=password)

    # Abrir sesión interactiva
    shell = client.invoke_shell()
    shell.recv(1024)  # Leer mensaje de bienvenida

    low, high = 1, 1000
    while low <= high:
        mid = (low + high) // 2
        shell.send(f"{mid}\n")  # Enviar número
        output = shell.recv(1024).decode()
        print(output)
        
        if "too high" in output:
            high = mid - 1
        elif "too low" in output:
            low = mid + 1
        elif "Correct!" in output:
            print("¡Número encontrado!", mid)
            break
    
    client.close()

if __name__ == "__main__":
    binary_search_ssh(
        host="atlas.picoctf.net", 
        port=50561, 
        username="ctf-player", 
        password="6dd28e9b"
    )

```

Errores detectados:
```
Traceback (most recent call last):
  File "C:\Users\korev\Desktop\busqueda_binaria.py", line 30, in <module>
    binary_search_ssh(
  File "C:\Users\korev\Desktop\busqueda_binaria.py", line 6, in binary_search_ssh
    client.connect(host, port=port, username=username, password=password)
  File "C:\Users\korev\AppData\Local\Programs\Python\Python312\Lib\site-packages\paramiko\client.py", line 409, in connect
    raise NoValidConnectionsError(errors)
paramiko.ssh_exception.NoValidConnectionsError: [Errno None] Unable to connect to port 50561 on 18.217.83.136
```


# binhexa

How well can you perfom basic binary operations?
Start searching for the flag here `nc titan.picoctf.net 61056`

1. Conectar por nc
	```
	nc titan.picoctf.net 61056
	```

2. Resolver las operaciones usando un código en Python.
	```python
	def bin_operations(a, b, operation):
	    if operation == '+':
	        result = bin(a + b)
	    elif operation == '-':
	        result = bin(a - b)
	    elif operation == '*':
	        result = bin(a * b)
	    elif operation == '>>':
	        result = bin(b >> 1)
	    elif operation == '<<':
	        result = bin(a << 1)
	    elif operation == '&':
	        result = bin(a & b)
	    elif operation == '|':
	        result = bin(a | b)
	    else:
	        result = "Invalid operation"
	    return result[2:]  
	
	a = int('11010111', 2)
	b = int('10011000', 2)
	operations = ['<<', '>>', '|', '&', '*', '+']
	for op in operations:
	    result = bin_operations(a, b, op)
	    print(f"Binary result for {op}: {result}")
	
	final_result = '111111110101000'
	print(f"Hexadecimal result: {hex(int(final_result[2:], 2))}")
	```

3. Obtener la bandera
	```
	SailorK3-picoctf@webshell:~$ nc titan.picoctf.net 61056
	
	Welcome to the Binary Challenge!"
	Your task is to perform the unique operations in the given order and find the final result in hexadecimal that yields the flag.
	
	Binary Number 1: 11010111
	Binary Number 2: 10011000
	
	
	Question 1/6:
	Operation 1: '&'
	Perform the operation on Binary Number 1&2.
	Enter the binary result: 10010000
	Correct!
	
	Question 2/6:
	Operation 2: '>>'
	Perform a right shift of Binary Number 2 by 1 bits .
	Enter the binary result: 1001100
	Correct!
	
	Question 3/6:
	Operation 3: '+'
	Perform the operation on Binary Number 1&2.
	Enter the binary result: 101101111
	Correct!
	
	Question 4/6:
	Operation 4: '|'
	Perform the operation on Binary Number 1&2.
	Enter the binary result: 11011111
	Correct!
	
	Question 5/6:
	Operation 5: '<<'
	Perform a left shift of Binary Number 1 by 1 bits.
	Enter the binary result: 110101110
	Correct!
	
	Question 6/6:
	Operation 6: '*'
	Perform the operation on Binary Number 1&2.
	Enter the binary result: 111111110101000
	Correct!
	
	Enter the results of the last operation in hexadecimal: 7fa8
	
	Correct answer!
	The flag is: picoCTF{b1tw^3se_0p3eR@tI0n_su33essFuL_d9a7ddd2}
	```


# ASCII Numbers

Convert the following string of ASCII numbers into a readable string:`0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d`

CyberChef is a great tool for any encoding but especially ASCII.
Try CyberChef's 'From Hex' function

```
recipe: From Hex

input: 0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x35 0x63 0x31 0x31 0x5f 0x6e 0x30 0x5f 0x71 0x75 0x33 0x35 0x37 0x31 0x30 0x6e 0x35 0x5f 0x31 0x6c 0x6c 0x5f 0x74 0x33 0x31 0x31 0x5f 0x79 0x33 0x5f 0x6e 0x30 0x5f 0x6c 0x31 0x33 0x35 0x5f 0x34 0x34 0x35 0x64 0x34 0x31 0x38 0x30 0x7d

output: picoCTF{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_445d4180}
```




