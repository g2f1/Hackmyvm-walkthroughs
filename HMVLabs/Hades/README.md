# Level 1

we use the initial credentials to connect to the machine

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
| Row 1    | Row 1    |h




