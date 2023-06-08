# web/an-inaccessible-admin-panel

## By Ketchup306

## Description

The Joker is on the loose again in Gotham City! Police have found a web application where the Joker had allegedly tampered with. This mysterious web application has login page, but it has been behaving abnormally lately. Some time ago, an admin panel was created, but unfortunately, the password was lost to time. Unless you can find it...

Can you prove that the Joker had tampered with the website?

Default login info: Username: default Password: password123

Link to login page: https://login-web-challenge.hsctf.com/

## Solution

By visiting the webpage we are presented with the login screen but to get the flag we need to combine the Admin password with the Admin username.

After cleaning up the javascript on the client side we get this

````javascript
function fii(num){
    return num / 2 + fee(num);
}
function fee(num){
    return foo(num * 5, square(num));
}
function foo(x, y){
    return x*x + y*y + 2*x*y;
}
function square(num){
    return num * num;
}

var key = [32421672.5, 160022555, 197009354, 184036413, 165791431.5, 110250050, 203747134.5, 106007665.5, 114618486.5, 1401872, 20702532.5, 1401872, 37896374, 133402552.5, 197009354, 197009354, 148937670, 114618486.5, 1401872, 20702532.5, 160022555, 97891284.5, 184036413, 106007665.5, 128504948, 232440576.5, 4648358, 1401872, 58522542.5, 171714872, 190440057.5, 114618486.5, 197009354, 1401872, 55890618, 128504948, 114618486.5, 1401872, 26071270.5, 190440057.5, 197009354, 97891284.5, 101888885, 148937670, 133402552.5, 190440057.5, 128504948, 114618486.5, 110250050, 1401872, 44036535.5, 184036413, 110250050, 114618486.5, 184036413, 4648358, 1401872, 20702532.5, 160022555, 110250050, 1401872, 26071270.5, 210656255, 114618486.5, 184036413, 232440576.5, 197009354, 128504948, 133402552.5, 160022555, 123743427.5, 1401872, 21958629, 114618486.5, 106007665.5, 165791431.5, 154405530.5, 114618486.5, 190440057.5, 1401872, 23271009.5, 128504948, 97891284.5, 165791431.5, 190440057.5, 1572532.5, 1572532.5];

function validatePassword(password){
    var password_array = password.split('').map(function(char) {
        return char.charCodeAt(0);
    });
    var checker = [];
    for (var i = 0; i < key.length; i++) {
        var password_letter = key[i];
        var proccessed_password_letter = inverse(password_letter);
        checker.push(proccessed_password_letter);
    }
    console.log(checker);

    if (key.length !== checker.length) {
        return false;
    }

    for (var i = 0; i < key.length; i++) {
        if (key[i] !== checker[i]) {
            return false;
        }
    }
    return true;
}

validatePassword('password');
````

We can imediatly see that the key is the array of numbers and we can deduce that we probably need to reverse this.

After a while of tinkering with numworks calculator I was able to get this equation:

`x/2 + x*5*x*5 + x*x*x*x + 2*x*5*x*x = key[i]`

We can use a very popular `sympy` library to solve equations like that in python

````
key = [32421672.5, 160022555, 197009354, 184036413, 165791431.5, 110250050, 203747134.5, 106007665.5, 114618486.5, 1401872, 20702532.5, 1401872, 37896374, 133402552.5, 197009354, 197009354, 148937670, 114618486.5, 1401872, 20702532.5, 160022555, 97891284.5, 184036413, 106007665.5, 128504948, 232440576.5, 4648358, 1401872, 58522542.5, 171714872, 190440057.5, 114618486.5, 197009354, 1401872, 55890618, 128504948, 114618486.5, 1401872, 26071270.5, 190440057.5, 197009354, 97891284.5, 101888885, 148937670, 133402552.5, 190440057.5, 128504948, 114618486.5, 110250050, 1401872, 44036535.5, 184036413, 110250050, 114618486.5, 184036413, 4648358, 1401872, 20702532.5, 160022555, 110250050, 1401872, 26071270.5, 210656255, 114618486.5, 184036413, 232440576.5, 197009354, 128504948, 133402552.5, 160022555, 123743427.5, 1401872, 21958629, 114618486.5, 106007665.5, 165791431.5, 154405530.5, 114618486.5, 190440057.5, 1401872, 23271009.5, 128504948, 97891284.5, 165791431.5, 190440057.5, 1572532.5, 1572532.5];

from sympy import symbols, Eq, solve

flag = ''
for i in range(len(key)):
    x = symbols('x')
    equation = Eq(x/2 + x*5*x*5 + x*x*x*x + 2*x*5*x*x, key[i])
    solution = solve(equation, x)
    if solution[0] < 0:
        solution[0] = solution[1]
    try:
        flag += chr(int(solution[0]))
    except:
        flag+= '@'

print(flag)
````

After running it we get output:

``Introduce A Little Anarchy, Upset The Established Order, And Everything Becomes Chaos!!``

Thanks to Ketchup306 for this amazing challenge

# FLAG CAPTURED!