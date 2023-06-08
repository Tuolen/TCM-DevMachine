# TCM-DevMachine Walkthrough

we found http on ports 80, 8080 and we found also nfs service.
- Target address is 10.0.2.8

---

Let's try to check for nfs
```bash
showmount -e 10.0.2.8
```
we got this :

---
1. We try to mount the file /srv/nfs after creating dev directory in /mnt:
```bash
mount -t nfs 10.0.2.8:/srv/nfs /mnt/dev 
# requires sudo
```
2. we got this file but it is locked with a password, so we use fcrackzip tool
3. we use fcrackzip to intiates a dictrionary attack on that zip file to crack the password
```bash
fcrackzip -v -u -D -p /usr/share/wordlists/rockyou.txt save.zip
```
4. the result
5. now we will unzip the file with the password we got, resulting in have id_rsa and todo.txt files

although we can use the id_rsa which is a private key to login using ssh we don't really know the username that we will use and if we try jp it won't work.

---
We used ffuf to go through directories that are in the webservers in both port 80 and 8080
```bash
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.0.2.8//FUZZ
# apply it also for port 8080 by adding doing it like this --> http://10.0.2.8:8080
```

---
We found this page through dirbuster and ffuf
let's create an accuont.

---
there is a local file inclusion vulnerability in boltwire, but you should be logged in to use and that's why we created an account in the beginning.

and after we apply the exploit

we got the /etc/passwd and now we can check for the users in the system.
and we found that jeanpaul is a user in the system and his name correlates with jp and loving of java and the username and the password we found in config.yml file.


---
we know that the password is "I_love_java", and we have the private key (id_rsa), so we connect to the user jeanpaul and we are finally in.

---
let's do " sudo -l "
as we can see we can use sudo with zip without password so we need to exploit this to gain root privilages.
now we apply this

---
WE BECAME ROOT!
