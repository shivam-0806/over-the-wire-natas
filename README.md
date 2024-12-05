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
play around. type * and it gives the source code. tried ls -a. # verifies that it's working. ```| cat``` gives entire txt file. or just do http://natas9.natas.labs.overthewire.org/dictionary.txt (as proven by typing ```ls -a``` in search)<br> given at beginning of natas - all pwds are at /etc/natas_webpass/natasX. so type ```| cat /etc/natas_webpass/natas10```.<br><br>
->Natas10<br>
search ```"" cat /etc/natas_webpass/natas11```.<br><br>
->Natas11<br>
WTF. 3 things - key,cipher,plaintext - are reqd. in XOR encryption. to find key do XOR of cipher and plaintext. plaintext = ```{"showpassword":"no","bgcolor":"#ffffff"}```. copy COOKIE, b64 decode it, XOR it using plaintext as key. key will be obtained as repeated pattern. now set showpwd to yes in plaintext and do reverse to find cookie. change value of cookie and reload page.<br><br>
->Natas12<br>
```A malicious file such as a Unix shell script, a windows virus, an Excel file with a dangerous formula, or a reverse shell can be uploaded on the server in order to execute code by an administrator or webmaster later – on the victim’s machine.```




