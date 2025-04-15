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



## 