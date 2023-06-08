# rev/micrurus-fulvius

## By hmmm

## Description

Micrurus fulvius, commonly known as the eastern coral snake, common coral snake, American cobra, and more, is a species of highly venomous coral snake in the family Elapidae. The species is endemic to the southeastern United States. It should not be confused with the scarlet snake (Cemophora coccinea) or scarlet kingsnake (Lampropeltis elapsoides), which are harmless mimics No subspecies are currently recognized.

Attachments:
[micrurus-fulvius.pyc](https://hsctf-10-resources.storage.googleapis.com/uploads/c86c0946f48499bc7e85bafc4daaac95031655f3b9685e907afd57608997cfaa/micrurus-fulvius.pyc)

## Solution

As we can see from the attached file, it is a python compiled file. We can use uncompyle6 to decompile it and clean it up a bit ourselves.

```python
from hashlib import sha256 as k

def a(n):
    b = 0
    while n != 1:
        if n & 1:
            n *= 3
            n += 1
        else:
            n //= 2
        b += 1

    return b


def d(u, p):
    return (u << p % 5) - 158


def j(q, w):
    return ord(q) * 115 + ord(w) * 21


def t():
    x = input()
    l = [-153, 462, 438, 1230, 1062, -24, -210, 54, 2694, 1254, 69, -162, 210, 150]
    m = 'b4f9d505'
    if len(x) - 1 != len(l):
        return False
    for i, c in enumerate(zip(x, x[1:])):
        if d(a(j(*c) - 10), i) * 3 != l[i]:
            print(f"Incorrect {i}    {c}")
            return False
        print(f"Correct {i}    {c}")
    if k(x.encode()).hexdigest()[:8] != m:
        return False
    return True


def g():
    if t():
        print('Correct')
    else:
        print('Wrong')


if __name__ == '__main__':
    g()
```

### Code explanation:

Immediately after decompiling we can see that the main function is function `t()`. It takes in an input from the console and sets it to value `x`.
Then it compares the length of the input and the length of the array `l`. So we can imediatly think that it is probably comparing it to the flag length and the array `l` is the encripted flag.

Then we see that it loops over an array where the pairs of x[i] and x[i+1] are together. Then it does some weird stuff calling many functions where the functions do something and return a number which is then compared to array `l`.

Then if all numbers in array `l` match processed letters in our input we proceed onto a sha256 has that is then converted to hex and the last 8 characters are compared to `b4f9d505`

If all this passed it means we got the correct flag.

### My procedure

This can really easily be bruteforced. But to do that we need to modify the script sligtly.

```python
chars = '}{-=!@#$%^&*()_+abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'
chars = list(chars)
from hashlib import sha256 as k

from mic import check_input

def a(n):
    b = 0
    while n != 1:
        if n & 1:
            n *= 3
            n += 1
        else:
            n //= 2
        b += 1

    return b


def d(u, p):
    return (u << p % 5) - 158


def j(q, w):
    return ord(q) * 115 + ord(w) * 21


def t(inp):
    x = inp
    l = [-153, 462, 438, 1230, 1062, -24, -210, 54, 2694, 1254, 69, -162, 210, 150]
    m = 'b4f9d505'
    guessed = 0
    for i, c in enumerate(zip(x, x[1:])):
        if d(a(j(*c) - 10), i) * 3 != l[i]:
            return guessed
        guessed += 1
    return guessed

def brute_force():
    possible_flags = ["fl"]
    while True:
        next_possible_flags = []
        for current_flag in possible_flags:
            for char in chars:
                if t(current_flag + char) == len(current_flag):
                    next_possible_flags.append(current_flag + char)

        possible_flags = next_possible_flags
        print(possible_flags)

brute_force()
```

### Code explanation

We could kind of guess that the first letters are `flag{` since it is the ctf flag format. 

* Notice how I removed length check since it would interfeer with our bruteforce.

In the function `bruteforce` We start with only one possible flag where the start is `fl` then while we start a never ending loop we iterate over the current possible flags and for every character we check the function `t` where if we get a result that we guessed is longer than the current flag it means we guessed one more character (this is a mouthful)

An in the end because the while loop never breaks we end up on an exception where the list `l` doesn't have enough characters to be compared against so it crashed. But it means we obtain the flag.

After running:

```
['fla']
['flab', 'flae', 'flaf', 'flag', 'flaM', 'flaZ']
['flag}', 'flag{', 'flagz']
['flag}(', 'flag{3', 'flag{5', 'flag{7', 'flagz9']
['flag}(m', 'flag}(r', 'flag}(v', 'flag{36', 'flag{38', 'flag{5-', 'flag{5(', 'flag{5+', 'flag{51']
['flag{366', 'flag{38-', 'flag{38+', 'flag{38L', 'flag{38M', 'flag{380', 'flag{5-f', 'flag{5-I', 'flag{5-K', 'flag{5(f', 'flag{5(h', 'flag{5(n', 'flag{5+t', 'flag{5+M', 'flag{5+Q', 'flag{5+U', 'flag{5+V', 'flag{5+W', 'flag{51=', 'flag{51O', 'flag{513']
['flag{3666', 'flag{38-f', 'flag{38-I', 'flag{38-K', 'flag{38+t', 'flag{38+M', 'flag{38+Q', 'flag{38+U', 'flag{38+V', 'flag{38+W', 'flag{380w', 'flag{380x', 'flag{380B', 'flag{380H', 'flag{3808', 'flag{3809', 'flag{513!', 'flag{513(']
['flag{3666&', 'flag{3666F', 'flag{3666M', 'flag{3808=', 'flag{3808*', 'flag{38082', 'flag{38087', 'flag{3809%', 'flag{3809)', 'flag{38090', 'flag{38092']
['flag{3666&r', 'flag{3666&F', 'flag{3666&V', 'flag{3666&8', 'flag{3808*@', 'flag{3808*(', 'flag{3808*0', 'flag{380822', 'flag{3809%@', 'flag{3809%f', 'flag{3809%G', 'flag{3809%M', 'flag{3809%P', 'flag{3809%X', 'flag{3809)b', 'flag{3809)B', 'flag{3809)E', 'flag{3809)7', 'flag{38090=', 'flag{380922']
['flag{3808229', 'flag{3809229']
['flag{3808229c', 'flag{3808229l', 'flag{3808229D', 'flag{3808229E', 'flag{3808229M', 'flag{38082297', 'flag{3809229c', 'flag{3809229l', 'flag{3809229D', 'flag{3809229E', 'flag{3809229M', 'flag{38092297']
['flag{38082297-', 'flag{38082297!', 'flag{38082297$', 'flag{38082297%', 'flag{38082297f', 'flag{380822974', 'flag{38092297-', 'flag{38092297!', 'flag{38092297$', 'flag{38092297%', 'flag{38092297f', 'flag{380922974']
['flag{380822974}', 'flag{380922974}']
Traceback (most recent call last):
  File "####", line 53, in <module>
    brute_force()
  File "####", line 47, in brute_force
    if t(current_flag + char) == len(current_flag):
       ^^^^^^^^^^^^^^^^^^^^^^
  File "####", line 34, in t
    if d(a(j(*c) - 10), i) * 3 != l[i]:
```

We can see how we ended up with 2 flags where I just tested them both and found the correct one.

Thank you hmmm for this awesome challenge.

# FLAG CAPTURED!