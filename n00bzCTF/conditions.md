# web/conditions

## By NoobMaster

## Description

Come and get the flag... Only if you can pass these impossible to pass conditions! Author: NoobMaster

Attachments:
[server.py](https://ctf.n00bzunit3d.xyz/files/3135463b4380538240d9aa1b82765850/server.py)

## Solution


server.py
```python
from flask import Flask, request, render_template, render_template_string, redirect
import subprocess
import urllib
flag = open('flag.txt').read()
app = Flask(__name__)
@app.route('/')
def main():
    return redirect('/login')

@app.route('/login',methods=['GET','POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    elif request.method == 'POST':
        if len(request.values["username"]) >= 40:
            return render_template_string("Username is too long!")
        elif len(request.values["username"].upper()) <= 50:
            return render_template_string("Username is too short!")
        else:
            return flag
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
```

From the server code we can see that we need to bypass an impossible condition. The length of our username has to be longer then `50` but shorter then `40`

We can notice that there is a `.upper()` in a second condition which gave me an idea... maybe there are some characters that double when they are put under `.upper()`.

Surely enough the german `ß` gets converted to `SS` when put under `.upper()` which means if we put `ßßßßßßßßßßßßßßßßßßßßßßßßßßßßßß` as our username we will bypass both conditions and get the flag.

Thank you NoobMaster for this amazing challenge :).

# FLAG CAPTURED!