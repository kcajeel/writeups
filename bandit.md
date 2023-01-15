---
title: OverTheWire Bandit
author: by Jack
date: 4/25/2021 to 5/1/2021
---
 
# Bandit 0
 
- I found a password in the readme file under bandit0:  
  Password: `boJ9jbbUNNfktd78OOpsqOltutMc3MY1`
 
# Bandit 1
 
- bandit1 has a dashed file, which takes stdin when opened. I redirected it into cat to get the password:  
  `$ cat < -`
  Password: `CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9`
 
# Bandit 2
 
- This one has a file called "spaces in this filename" I just opened it with cat, using the tab autocomplete.  
  And it escaped the spaces with "\ " so it wasn't as challenging as I think it was supposed to be.  
   `The password is UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK`
 
# Bandit 3
 
- There is a directory called "inhere" and no apparent files under it.  
- But, you can look for hidden files with `$ ls -a` and you see a file aptly named ".hidden" with the password:  
  `pIwrPrtPN36QITSp3EQaw936yaFoFgAB`
 
# Bandit 4
 
- This one was hard but once you figure out how to do it, it's fun.  
- one of the hints they gave you is to use the `file` command, which shows filetypes.  
- Since it won't run on any of the files because they have dashes in their names, you have to type `$ file ./[FILENAME]`  
- When you go through them, you'll see that most of them are listed as "data" but -file07 is listed as "ASCII text"  
- because the name has a dash, you need to redirect the cat like you did in Bandit 1 to get the password:  
  `koReBOKuIDDepwhWk7jZC0RTdopnAYKh`
 
# Bandit 5
 
- This one is intimidating at first, but it's not that difficult.  
- you need to look at the OTW info page for this one tho.  
- they tell you that the file is:  
  - human-readable, 1033 bytes in size, and not executable  
- You could spend all day looking for it, or you could use the `find` command.  
- run the command `$ find -type f -size 1033c` which looks for files that are normal readable files and are 1033 bytes.  
  - The password is in the file and it is `DXjZPULLxYr17uwoI01bNLQbtFemEgo7`
 
# Bandit 6
 
- Again, this one seems daunting but it's really just a simple command.  
- The ebsite says the password is stored somewhere on the machine and has the following characteristics:  
  - It's owned by user bandit7 and group bandit6, and 33 bytes in size.  
- It already sounds a lot like bandit 5, so we're probably using `find` again.  
- after some digging in the find manpage, I forged the following command:  
  `$ find -user bandit7 -group bandit6 -size 33c` and it worked!  
- the password is under "./var/lib/dpkg/info/bandit7.password":  
  `HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs`
 
# Bandit 7
 
- the website says the passwod is in data.txt next to the word millionth.  
- They seem to want you to use one of the commands they reccomend.  
- I just read the file with `less` and searched it.  
  - less is like cat, but it's better for longer files bc you can scroll in it and search for words with `/[TERM]`  
- I'm pretty sure manpages are automatically read in less.  
- Anyways, I typed `/millionth` and then i found the password within like 3 seconds of starting this level:  
  `cvX2JJa4CFALtqS87jk27qwqGhBM9plV`
 
# Bandit 8
 
- the website says that this password is stored in data.txt and is the only line that occurs only once.  
- just by looking at the reccomended commands, I'm pretty sure that `uniq` will solve this one.  
- I tried runnint `$ uniq -u data.txt` but that didn't work. I read the manpage a little more and found that uniq only compares adjacent lines.  
- I think that if I can get all of the repeating lines next to each other, and then run `$ uniq -u` I'll find the password.  
- I think `sort` will work, so it tried `$ sort data.txt`  
- It prints every line, so I could run '`$ sort data.txt > sorted.txt` and then run uniq on sorted.txt  
- or I could do it all in one go with `$ sort data.txt | uniq -u`  
  - the pipe takes the output from the first command and uses it as input for the second.  
  `The password is UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR`
 
# Bandit 9
 
- This one says that the password is stored in "data.txt" and is human readable, preceded by a series of = characters.  
- I solved this one with a less search, but `$ cat data.txt | grep -a ===` works too.  
  - you need grep -a bc grep recognizes it as a binary file. -a allows grep to work on the binary.  
- In less, you can type `n` to see the next instance of the search and `N` to see a previous instance.  
- The file has, in different locations, the phrase:  
  - `the password is: truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk`
 
# Bandit 10
 
- The hint is that the password is encoded in base64.  
- So, I looked through the manpage for the command `base64` and made this command:  
  `$ base64 -d data.txt`  
- it decoded data.txt, which is encoded in base64 as I said earlier. The message I got was:  
  `The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR`
 
# Bandit 11
 
- This password is encoded in ROT13, where every character is substituted with the one 13 places ahead of it.  
- I looked at manages for a lot of the reccomended commands and decided that `tr` for translate, would work.  
- The command works by translating characters from the first set into the second set.  
- In ROT13, A would become N, and because 26/2=13, rotating N by 13 one more time would undo the cipher and output A.  
- So we decrypt the text by encrypting it again with the following command:   
  `$ cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]`  
- This command basically turns everything in the capital and lowecase alphabet forward by 13 characters.  
- The result is the password for the next level:  
  `The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu`
 
# Bandit 12
 
- This one took a while.  
- The hint is that the file has been encoded as a hexdump, and compressed many times  
- It also tells you to make a directorey under /tmp/ which I did.  
- To decode the hex, I ran `$ xxd -r data.txt 12`  
- this uses xxd to decode the hexdump and output it into a new file called "12" for Bandit 12  
- Then I wasn't sure what to do with it, so I ran `file` on 12.  
- The file was gzipped, so i added the .hz extension with `$ mv 12 12.gz`  
- Then ran `$ gzip -d 12.gz` and it decompressed.  
- I ran `file` again and found that it was compressed with bzip2  
- so i added the .bz2 extension to it the same way I added .gz earlier  
- I ran `$ bzip2 -d 12.bz2` to extract it and ran file again  
- Now it's another gzip, so I repeated the first part  
- I ran file again. It's a tar archive.  
- I extracted it with `$ tar --extract -f 12` and it gave me a new file called "data5.bin"  
- I ran file on data5.bin to find that it was another tar archive  
- I extracted it and it gave me data6.bin, which was compressed with bzip2  
- I decompressed data6.bin and ran file again. It was a tar archive.  
- I decompressed data6 to get data8.bin, which was compressed with gzip.  
- I unzipped data8.bin and ran file on the resulting data8 file  
- It's ASCII text!! It says:  
  `The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL`  
 
# Bandit 13
 
- This lab deals with ssh keys. I have a privkey for bandit 14, who has access to my password.  
- I ran `$ ssh -i bandit14@localhost` this uses the identity of the privkey to access bandit14's account.  
- They told me the password was in /etc/bandit_pass//bandit14, and it was:  
  `4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e`  
- Since I'm already logged in as bandit 14, I won't use it though.  
 
# Bandit 14
 
- This one was easy. I just had to pass the password for bandit 14 I got from the ast one to port 30000  
- to do this, I scanned the localhost with `$ nmap localhost` and found the following:  
  ```
  PORT      STATE SERVICE
  22/tcp    open  ssh
  113/tcp   open  ident
  30000/tcp open  ndmps
  ```
- Port 30000 uses ndmps, which I can send data to with netcat.  
- I ran `$ nc localhost 30000` to connect to it, and when I connected I pasted the password.  
- The netcat session had the following output:  
  `Correct!`   
  `BfMYroe26WYalil77FoDi9qh59eK5xNr`  
 
# Bandit 15
 
- This is another one that seems hard, but isn't.  
- The hint says that I need to pass the current password to port 30001 using SSL encryption.  
- I looked through the manpage of s_client and made the following command:  
  `$ openssl s_client -connect localhost:30001`  
- It connected me and I pasted the password. It output this:  
  `Correct!`   
  `cluFn7wTiGryunymYOu4RcffSxQluehd`  
 
# Bandit 16
 
- For this lab, I have to send the current password to some port between 31000 and 32000.  
- I ran `$ nmap -p 31000-32000 localhost` to scan the port range. It gave me:  
  ```
  PORT  STATE SERVICE
  31046/tcp open  unknown
  31518/tcp open  unknown
  31691/tcp open  unknown
  31790/tcp open  unknown
  31960/tcp open  unknown
  ```
- I ran `$ nmap -sV -p 31000-32000 localhost` to determine the services on those ports:  
  ```
  PORT      STATE SERVICE     VERSION
  31046/tcp open  echo
  31518/tcp open  ssl/echo
  31691/tcp open  echo
  31790/tcp open  ssl/unknown
  31960/tcp open  echo
  ```
- I connected to port 31790 with   
  `$ opensssl s_client -connect localhost:31790`  
- I input the password and it gave me:  
  ```
  Correct!
    -----BEGIN RSA PRIVATE KEY-----
    MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
    imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
    Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
    DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
    JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
    x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
    KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
    J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
    d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
    YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
    vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
    +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
    8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
    SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
    HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
    SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
    R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
    Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
    R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
    L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
    blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
    YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
    77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
    dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
    vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
    -----END RSA PRIVATE KEY-----
  ```
- this is obviously an SSH privkey. I made the key into a file in the /tmp/ directory  
- when I tried to connect, it told me that the permissions were too high, s I changed them  
- `$ chmod 700 key.txt` makes it so nobody else can access the file  
- it worked.  
 
# Bandit 17
 
- There are two files, passwords.old and passwords.new  
- The password for the next level is the only line that is different between the two files.  
- I ran `$ diff passwords.old passwords.new` to see the differences, and found the key.  
  `kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`  
 
# Bandit 18
 
- this lab kicks you out when you try to login normally.  
- in the ssh manpage, I found `-T` which disables pseudo-terminal allocation.  
- It looks like that's what's happening, so I ran `$ ssh -T bandit18@localhost`  
- it worked, but there isn't a terminal prompt so it looks blank.  
- the password is stored in the readme file:  
  `IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`  
 
# Bandit 19
 
- it says there is a setuid binary in my home directory. There is, called "bandit20-do"  
- I ran `file` on it and it came back as a setuid executable. I executed it with "./bandit20-do" and it said to use the "id" argument.  
- I ran `$ ./bandit20-do id` and got that bandit20's uid is 11020  
- I tried `$ ./bandit20-do ls` and figured out that executing the binary does commands as the user bandit20, like the name implies  
- I ran `$ ./bandit20-do cat /etc/bandit_pass/bandit20` and it gave me:  
  `GbKksEFF4yrVs6il55v6gwY5aVje5f0j`  
 
# Bandit 20
 
- This time, there is a setuid called "suconnect" that connects to a specified port, and if given the current password, outputs the next one.  
- It also said to use screen, which I use on my mc server.  
- I set up a screen session called nc with `$ screen -t nc`  
- I setup a nc listener on port 6969 `$ nc -lp 6969`  
- i exited my screen session `CTRL+A+D` and ran `$ ./suconnect 6969`  
  - it didn't work  
- I remembered I had to give it the curent password  
- I recconected to my screen session `screen -r`  
- I decided to pipe it with echo `$ echo [PASSWORD] | nc -lp 6969`  
- I then ran `$ ./suconnect 6969`  
  - it said "Password matches, sending next password"  
- In the nc screen, it gave me the next password:  
  `gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr`  
 
# Bandit 21
 
- it says there is a cronjob running, and to look at "/etc/cron.g/"  
- I found a cronjob called "bandit22"  
- it ran a script called "cronjob_bandit22.sh"  
	```
	#!/bin/bash
	chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
	cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
	```
- This script makes it so I can read the /tmp/ file, and then outputs the bandit22 password to it.   
- I ran `$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` and it gave me the password: `Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI`  
 
# Bandit 22
 
- These cron scripts are fun to do. This one is just like the last one, here's the script:  
  ```  
  #!/bin/bash
   myname=$(whoami)
   mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
   echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget
  ```
- this script sets up two variables, myname and mytarget.  
- it then copies the password from myname into the file /tmp/mytarget  
- mytarget is calculated as an md5 hash with some stuff cut off  
- I ran the command `$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1`  
- it gave me: `8ca319486bfbbc3663ea0fbe81326349`  
- I catted /tmp/8ca319486bfbbc3663ea0fbe81326349 to find the password:  
  `jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n`  
 
# Bandit 23
 
- this is another cron script. Here's what it says:  
  ```
  #!/bin/bash
  myname=$(whoami)
  cd /var/spool/$myname
  echo "Executing and deleting all scripts in /var/spool/$myname:"
  for i in *.*;
  do
      if [ "$i" != "." -a "$i" != ".." ];
      then
          echo "Handling $i"
          owner="$(stat --format "%U" ./$i)"
          if [ "${owner}" = "bandit23" ]; then
              timeout -s 9 60 ./$i
          fi
          rm -f ./$i
      fi
  done
  ```
- Basically, it checks in the /var/spool/bandit24 directory for files owned by bandit23.  
- It runs and deletes them after a minute (timeout -s 9 60 ./$1)  
  - the part about the files named "." and ".." are for those files, which are hidden and exist in every Linux directory.  
  - the script skips over them so it doesn't remove them  
- I cd'd into /var/spool/bandit24 and made a really simple test script  
  ```   
  #!/bin/bash
   echo it works
  ```
- I had to change permissions (chmod 777) but it worked, so I made a better script  
  ```
   #!/bin/bash
   cat /etc/bandit_pass/bandit24 > /tmp/test/pass.txt
   ```
- I had trouble, but then I remembered I needed to allow write access to pass.txt  
  - chmod 666 pass.txt (if you have trouble understanding my chmod commands, read my security+ notes)  
- after the longest minute ever, it worked:  
  `UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ`  
 
# Bandit 24
 
- There is a listening daemon on 30002, and it will give me the password if i send it the bandit24 password and a 4-digit code.  
- there isn't a hint for the code, I have to write a bruteforcing script.  
- i knew I would need a for loop to set up my pincode, but I wasn't sure how to implement it  
- I found this helpful [tutorial](https://ryanstutorials.net/bash-scripting-tutorial/bash-loops.php)  
- Here is my script:  
  ```  
  #!/bin/bash
  for i in {0000..9999}
   do
       echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ" $i
   done
  ```
- This verison just outputs the bruteforce and the password, to pass the output to the port I used the `>>` operand to append it to a file.  
  ```  
  #!/bin/bash
   for i in {0000..9999}
      do
          echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ" $i >> input.txt
      done
  ```
- Now I just need to pass it to the port with netcat:  
  `$ cat input.txt | nc localhost 30002`  
- The password is:  
  `uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG`  
 
# Bandit 25
 
- this level needs you to break into bandit26's account. It also says that their shell is not /bin/bash  
- I ran `$ cat /etc/passwd | grep bandit26` to find that their shell is /usr/bin/showtext  
- when I ran `$ cat /usr/bin/showtext` the following script appeared:  
  ```  
  #!/bin/sh
  export TERM=linux
  more ~/text.txt
  exit 0
  ```
- I was denied read access from /home/bandit26/text.txt  
- I have a bandit26 sshkey in my home directory, so I tried it:  
  `$ ssh -i bandit26.sshkey bandit26@localhost`  
- it kicks me out, but it prints a file (which is the script executing the 3rd line)  
  - full disclosure, I cheated just for this step b/c I didn't know how else to get into bandit26's shell  
  - You have to resize your terminal window so that the "bandit26" text isnt fully displayed.  
  - That way, it executes `more` on the text before exiting because more hasnt completed displaying yet.  
    - that was the only part I cheated on, I'll still use the internet after this line, but I won't search for anyting specific to bandit.  
- I read the manpage for more, and found that typing `v` inputs the file into vim.  
- I found [a writeup online about common exploits](https://www.exploit-db.com/docs/english/44592-linux-restricted-shell-bypass-guide.pdf) (i looked up "exploit more command")  
  - vim has one where you can spawn a bash shell, type `:!/bin/bash`   
- It just goes back to the more text, which means that it executes the shell for the account, not specifically bash  
- I looked up [how to change the shell in vim](https://vi.stackexchange.com/questions/19522/is-it-possible-to-change-the-default-terminal-of-vim), and you can do it with the command `:set shell=[SHELL]`  
- I want bash, so I set it to /bin/bash and now the exploit works.  
- like usual, I ran `$ cat /etc/bandit_pass/bandit26` and I got the password:  
  `5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z`  
 
# Bandit 26
 
- You can't login with ssh, you need to use the exploit from the previous lab to get bandit26's shell  
- there's a file in my home directory named "bandit27-do"  
- I catted it and it's a setuid binary. I checked persmissions with `$ ls -la` and I can execute it.  
- I ran `$ ./bandit27-do cat /etc/bandit_pass/bandit27` and it worked:  
  `3ba3118a22e93127a4ed485be72ef5ea`  
 
# Bandit 27
 
- It says there's a git repo at ssh://bandit27-git@localhost/home/bandit27-git/repo  
- I created a /tmp/ directory and ran `git clone` on the repo.  
- You need to use bandit27's password to clone it, but it works:  
  `The password to the next level is: 0ef186ac70e04ea33b4c1853d2526fa2`  
 
# Bandit 28
 
- there is another git repo, but the file I get from it doesn't really have a password:  
  ```  
  # Bandit Notes
  Some notes for level29 of bandit.
  ## credentials
  username: bandit29
  password: xxxxxxxxxx
  ```
- I tried to literally use that password, but it didn't work :(  
- The hint seemed pretty straightforward, so I think it's in this git repo somewhere  
- I ran `find -type f` to find all of the files in the repo directory  
- It gave me a fairly large list, and I wanted to cat all of them.  
- I looked up [how to read the list with a bash script](https://www.cyberciti.biz/faq/unix-howto-read-line-by-line-from-file/) 
- I modified their script to cat every file and output the result to "output.txt"  
  ``` 
  #!/bin/bash
  input="list.txt"
  while IFS= read -r line
   do
        cat "$line" >> output.txt
   done < "$input"
  ```
- the script worked, but the file didn't contain the password :(  
- The only reccomended command is `git` so I looked in the manpage to see if the readme was changed  
- if you just run `$ git` you can see common commands  
- `$ git commit` only showed my script files  
- `$ git branch` showed there was only the master branch  
- `$ git show` showed me the recent commits, among which was one that changed the readme  
- the password was obfuscated with the x's, and it was:  
  `bbc96594b4e001778eee9975372716b2`  
 
# Bandit 29
 
- this is another git repo problem. I cloned it.  
- the readme says the following:  
  ```
  # Bandit Notes
  Some notes for bandit30 of bandit.
  ## credentials
  username: bandit30
  password: no passwords in production!
  ```
- i ran `$ git show` and there is one commit from "Ben Dover" that changed the username in the readme from bandit29 to bandit30  
- I wasn't sure what to do so I ran `$ git --help` and found that I can use help on individual commands  
- I ran `$ git help []` on show, commit, and branch  
- there were only 2 commits, the one creating readme and the one changing it  
- there's more than one branch though. I found them with `$ git branch -a`  
- I wanted to switch branches, so I looked in the common commands list  
- I found `git checkout` which allows you to switch branches  
- I ran `$ git checkout sploits-dev` an there's an exploits directory but it didn't have anything for me  
- there's a README.md but it's the same as the master branch  
- the only other branch to check out is "dev" so I go there  
- there's a "gif2ascii.py" python file but it's empty  
- the readme is different! it has the password:  
  `5b90576bedb2cc04c86a9e924ce42faf`  
 
# Bandit 30
 
- yet another git repo problem  
- you know the drill, I git clone the repo and cat the readme:  
  ```
  just an epmty file... muahaha
  ```
- interesting. I checked for branches and it's just the master  
- `git show` only shows the initial commit of the readme.  
  - it's made by Ben Dover again lmaoo  
- I was messing with `git rebase` and when I tried tab autocompleting it showed there's a "secret" branch  
- I ran `$ git branch secret` and then `$ git checkout secret` and catted the readme  
- it's the same, but there's two new autocomplete options, heads/secret and tags/secret  
- I ran `$ git show` on each of them, and it gave me a password-looking output when I ran   
  `$ git show tags/secret`  
  `47e603bb428404d265f59c42920d81e5`  
- I tried to login to bandit31 with it and it worked!  
 
# Bandit 31
 
- same deal, git clone, cat README.md:  
  ```
  This time your task is to push a file to the remote repository.
  Details:
   File name: key.txt
   Content: 'May I come in?'
   Branch: master
  ```
- It wants me to make a file called "key.txt" with the text "May I come in?" and commit it  
- I made the file and then looked up [how to push a file to a git repo](https://www.datacamp.com/community/tutorials/git-push-pull)  
- I ran `$ git add -f key.txt` b/c it said that the file was one of my gitignore files  
  - so I needed to "force" it.  
- I ran `git status` and saw that it was ready to be committed  
  `$ git commit -m 'please work'`  
  `$ git push -u origin master` pushes the current master to the upstream repo  
- it worked! This one was really cool to do because I have no idea how they made this work. it's like magic:  
  ```
  remote: # Attempting to validate files... ##
  remote:
  remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
  remote:
  remote: Well done! Here is the password for the next level:
  remote: 56a9bf19c63d650ce78e6ec0354ee45e
  remote:
  remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
  remote:
  ```
- This level was really cool.  
 
# Bandit 32
 
- this one spawn into the "UPPERCASE SHELL" and everything is uppercase  
- I can't do anything in it, so I logged back in as bandit31 to try and enumerate  
- /etc/passwd says that bandit32 uses /home/bandit32/uppershell  
- I can't read or execute uppershell, but I ran `file` and it's a setuid file  
- I ssh'd into bandit32 again to explore more with the uppershell  
- the errors when typing commands are from "sh"  
- I can type environment variables like $SHELL, $USER, etc.  
- I can't do much more, so I look in the sh manpage  
  - I search for `\$` because the $ has to be escaped  
  - I found a section about "Special Parameters" around line 540  
- I ssh back and try typing the special parameters in.  
- $$ reveals that the PID is 9678  
- $0 gives me a really basic shell, and I can run commands!
- I ran `$ cat /etc/bandit_pass/bandit33` and the password is:  
  `c9c3199ddf4121b10cf581a98d51caee`  
 
# Bandit 33
 
- there is a README.txt:
  ```
  Congratulations on solving the last level of this game!  
  At this moment, there are no more levels to play in this game. However, we are constantly working  
  on new levels and will most likely expand this game with more levels soon.  
  Keep an eye out for an announcement on our usual communication channels!  
  In the meantime, you could play some of our other wargames.  
  If you have an idea for an awesome new level, please let us know!  
  ```
 
# Conclusion
 
This was a really fun (and challenging) activity, and I highly reccommend it for any experience level.   
I've been using linux fairly consistently for the past 2-3 years and even I learned new stuff doing this lab.  
If you're just starting out with linux, the first few labs are really good for teaching basic commands in a fun setting.  
It does get quite challenging, so I would say to do as many labs as you can and don't get discouraged if one of them seems really hard.  
For absolute beginners, I always reccommend [linux journey](https://notes.siira.io/).  
That's all I really have to say, thanks for reading and stay lit  
Jack  
