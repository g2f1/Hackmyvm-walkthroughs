# Mission 1

The initial credentials to connect to the machine

    Host: hades.hackmyvm.eu
    Port: 6666
    User: hacker
    Pass: begood!

Each time we will find a file `mission.txt` that gives us some what we need to in kind of puzzle.

    ################
    # MISSION 0x01 #
    ################

    ## EN ##
    User acantha has left us a gift to obtain her powers.

    ## ES ##
    La usuaria acantha nos ha dejado un regalo para obtener sus poderes.

I found also a file `readme.txt` that gives some informations like that the home of each user is in `/pwned/USER` and the file `flagz.txt` contain some kind of flag that we can insert in the official site to participate in the ranking.

From the content of `mission.txt` file we can assume that the next user is `acantha` and she left something for us that will give us her power. So let's first try `sudo -v` to see what we can run as who

![image](./assets/level1_noSudo.png)

I tried to search for a file that contains the word `gift` in its name.

![image](./assets/level1_searchForGift.png)

I found an executable file called `gift_hacker`, which I could run because I belong to the `hacker` group. After executing it, I obtained a shell with the **UID of `acantha`**. However, I could not access her home directory because the shell still had the **GID of `hacker`**.  

So I decided To search for all the files owned by `acantha`, I used the following command:

```bash
find / -user acantha -type f 2>/dev/null
```

This search revealed an interesting file: `/pazz/acantha_pass.txt`, which contained the password for acantha.

| user | password | flag
|------|----------|-----
| acantha|mYYLhLBSkrzZqFydxGkn|^CaEuVJtJjaCwZtuuAFD^

# Mission 2

    ################
    # MISSION 0x02 #
    ################

    ## EN ##
    The user alala has left us a program, if we insert the 6 correct numbers, she gives us her password!

    ## ES ##
    La usuaria alala nos ha dejado un programa, si insertamos los 6 numeros correctos, nos da su password!

The executable `guess` prompts for a PIN code. If the correct PIN is entered, it reveals the password for the user `alala`. if not, it simply returns *"No"   

To extract this password, we can either analyze the binary with a disassembler or the `strings` command

![image](./assets/level2_string.png)

| user | password | flag
|------|----------|-----
| alala|DsYzpJQrCEndEWIMxWxu|^gTdGmkwhDrCqKrDQpxH^

# Mission 3

    ################
    # MISSION 0x03 #
    ################

    ## EN ##
    User althea loves reading Linux help.

    ## ES ##
    A la usuaria althea le encanta leer la ayuda de Linux.

We need to find a way to read the file `althea_pass.txt` that's belong to the user and group althea. We have an executable `read` when excuting it we get the manuel page of the man command. But wait we can run commands from the man pages 

![image](./assets/level3_id.png)

So `read` set the uid to that od the user `althea`. We can use that to read the file containing the password

![image](./assets/level3_password.png)

| user | password | flag
|------|----------|-----
| althea|ObxEmwisYjERrDfvSbdA|^btDtPAPzSiXmoHItpqX^

# Mission 4

    ################
    # MISSION 0x04 #
    ################
    
    ## EN ##
    The user andromeda has left us a program to list directories.
    
    ## ES ##
    La usuaria andromeda nos ha dejado un programa para listar directorios.
    
I found a program called `lsme`. According to mission.txt, it is supposed to list directories. However, I wasn't able to list `/pwned/andromeda/` due to insufficient permissions. This suggests that the program sets the **UID** to that of `andromeda`, but leaves the **GID** unchanged, which restricts full access to the directory.


![image](./assets/level4_test.png)

The program asks for an input and then passes it directly to the `ls` command. The issue is that no validation or sanitization is performed on the input, which allows us to chain additional commands and potentially execute arbitrary commands.

![image](./assets/level4_cat.png)

| user | password | flag
|------|----------|-----
| andromeda|OTWGTbHzrxhYFSTlKcOt|^xzsHGrOeNctIZLGKzWq^

# Mission 5

    ################
    # MISSION 0x05 #
    ################
    
    ## EN ##
    The user anthea reminds us who we are.
    
    ## ES ##
    La usuaria anthea procura que no olvidemos quien somos.

This time we have a program  called  `uid` executing it with parm or withou them returns the result of the `id` commmand with the uid set to anthea. We have also like before a file that contains the password for `anthea`. 

![image](./assets/level5_uid.png)

Since the program call the command `id`, I create a **symlink** to the command `/bin/bash` with the name `id` and I add the `/tmp` dir in the variable `PATH`. This gives a shell with the uid of `anthea`.

![image](./assets/level5_bash.png)

| user | password | flag
|------|----------|-----
| anthea|yWFLtSNQArEBTHtWgkKd|^AcFLuAjhydNKIkPoFLL^

# Mission 6

    ################
    # MISSION 0x06 #
    ################
    
    ## EN ##
    User aphrodite is obsessed with the number 94. 
    
    ## ES ##
    La usuaria aphrodite esta obsesionada con el numero 94.

The program `obsessed` has weird behaviour. It takes an input from the environement variable MYID and return weird output.

![image](./assets/level6_1.png)

`strings` doesn't work. But it looks like it's about ascii(a encodes as 97 in ascii); so I need to put something that gives in ascii 94.

![image](./assets/level6_2.png)

| user | password | flag
|------|----------|-----
| aphrodite|HPJVaqRzieKQeyyATsFv|^fmPlsDByrwmEpRAKgeP^

# Mission 7

    ################
    # MISSION 0x07 #
    ################
    
    ## EN ##
    The user ariadne knows what we keep in our HOME.

    ## ES ##
    La usuaria ariadne sabe que es lo que guardamos en nuestro HOME.

This program(`homecontent`) execute the following `/bin/ls $HOME`. So I use the variable HOME to chain commands.

![image](./assets/level7.png)

| user | password | flag
|------|----------|-----
| ariadne|iNgNazuJrmhJKWixktzk|^FuGFaFNhtKNxUInxAtd^

# Mission 8

    ################
    # MISSION 0x08 #
    ################
    
    ## EN ##
    The user arete lets us use cp on her behalf. 
    
    ## ES ##
    La usuaria arete nos deja usar cp en su nombre.

Using `sudo -l` I confirm the `mission.txt` statement

![image](./assets/level8_sudo.png)

Now the task is clear and straightforward: access the home directory of `arete` and copy interesting files to a location where we have read permissions. The main question is what exactly needs to be copied. From previous missions, I know that in every home directory there is a file called `flagz.txt` which contains the flag (not the password). This is not an issue, since submitting the flag on the platform will reveal the corresponding password for that user.

![image](./assets/level8_flag.png)

| user | password | flag
|------|----------|-----
|arete|QjrIovHacmGWxVjXRLmA|^qmrrbGUXLTqLFDyCDlx^

# Mission 9

    ################
    # MISSION 0x09 #
    ################
    
    ## EN ##
    The user artemis allows us to use some binary on her behalf. Its a gift... 
    
    ## ES ##
    La usuaria artemis nos permite usar algun binario en su nombre. Es un regalo...

![image](./assets/level9_sudo.png)

/sbin/capsh is a handy wrapper for certain types of  capability testing and environment creation

It can be used to run a command.

![image](./assets/level9_capsh)

| user | password | flag
|------|----------|-----
|artemis|HIiaojeORLaJBVSPDDCZ|^SegGdzPgnNdGAmKjnsa^

# Mission 10

    ################
    # MISSION 0x10 #
    ################

    ## EN ##
    We need /bin/bash so that the user asia gives us her password. 

    ## ES ##
    Necesitamos /bin/bash para que la usuaria asia nos de su password.

When I run `restricted`, I directly obtain the password, but I’m not sure what the real purpose of this mission is. Maybe the idea is that I’m getting a regular Bash shell, while they actually expect us to spawn a different type of shell program.

| user | password | flag
|------|----------|-----
|asia|djqWtkLisbQlrGtLYHCv|^ngXdULWFWKCGtgxAQNv^

# Mission 11

    ################
    # MISSION 0x11 #
    ################

    ## EN ##
    The user asteria is teaching us to program in python. 

    ## ES ##
    La usuaria asteria nos esta enseñando a programar en python.

![image](./assets/level11_sudo)

python is runnable as the user asteria. We can exploit this to spawn a shell as `asteria`.

![image](./assets/level11_python)

| user | password | flag
|------|----------|-----
|asteria|hawMVJCYrBgoDAMVhuwT|^xSRhIftMsAwWvBAnqNZ^

# Mission 12

    ################
    # MISSION 0x12 #
    ################

    ## EN ##
    The user astraea believes in magic. 

    ## ES ##
    La usuaria astraea cree en la magia.

![image](./assets/level12_php)

The challenge is based on a simple PHP application that compares the hash of the input provided in the `pass` parameter with the hash of the real password of `astraea`.  
If the two match, the application returns the password in cleartext.  

They also mentioned something about *magic*, which immediately made me think of the **magic hash attack**.  
This vulnerability occurs when PHP performs a *loose comparison* (`==` instead of `===`), allowing certain specially crafted hash values (like those starting with `0e...`) to be interpreted as `0` and thus bypass the check.

I found a repo that contains a list of strings that gives magic hashes (`md5("QLTHNDT")=0e405967825401955372549139051580`)

![image](./assets/level12_php1)

| user | password | flag
|------|----------|-----
|astraea|nZkEYtjvHElOtupXKzTE|^KssHQIAFsxUamecyXIUk^

# Mission 13
