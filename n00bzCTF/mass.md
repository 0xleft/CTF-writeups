# crypto/MaaS

## By NoobMaster

## Description

Welcome to MaaS - Modulo as a Service! Author: NoobMaster

Attachments:
[chall.py](https://ctf.n00bzunit3d.xyz/files/b265c5538c1af3c1a8388b2359870ccf/chall.py)

## Solution

chall.py
```python
#!/usr/bin/python3
import random
from Crypto.Util.number import *
flag = open('flag.txt').read()
alpha = 'abcdefghijklmnopqrstuvwxyz'.upper()
to_guess = ''
for i in range(16):
	to_guess += random.choice(alpha)
for i in range(len(to_guess)):
	for j in range(3):
		inp = int(input(f'Guessing letter {i}, Enter Guess: '))
		guess = inp << 16
		print(guess % ord(to_guess[i]))
last_guess = input('Enter Guess: ')
if last_guess == to_guess:
	print(flag)
else:
	print('Incorrect! Bye!')
	exit()
```

Initialy I thought this was a random cracking (which I was really hoping for) challenge but after looking at it again it was clear that it wasn't.

## Code explanation

We can see that we have to guess a 16 letter sequence made up out of random characters. We have 3 guesses for each character. 

Since each input is shifted right by `16` and then modulo of it and the letter to guess is taken we can find out what was the original character.

```python
def find_letter(shifted_mod, number_used, possible_values):
    chars = 'abcdefghijklmnopqrstuvwxyz'.upper()
    temp_possible = []
    for char in chars:
        orded = ord(char)
        if (number_used << 16) % orded == shifted_mod:
            temp_possible.append(char)

    possible = []
    if len(possible_values) == 0:
        return temp_possible
    
    for char in temp_possible:
        if char in possible_values:
            possible.append(char)

    return possible
```

We can find the possible letter by looping over all the letters and shifting it by 16 and taking the modulo of the result and the character. If that is equal to the our result we are searching for we can add it to our possible character list.

Another think I noticed was that multiple characters had the same output so to try and mitigate that we can use the ``3`` tries we got to maximize our chances try using different numbers as inputs.

````python
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("challs.n00bzunit3d.xyz", 51081))
sock.recv(1024)
flag = ""
for i in range(16):
    possible_values = []
    for j in range(3):
        number_used = j + 1
        sock.send(f"{number_used}".encode() + b'\n')
        number = sock.recv(1024).decode().split('\n')[0]
        print(number)
        possible_values = find_letter(int(number), j + 1, possible_values)
    if len(possible_values) != 1:
        flag += possible_values[random.randint(0, len(possible_values) - 1)]
    else:
        flag += possible_values[0]

sock.send(flag.encode() + '\n'.encode())
answer = sock.recv(1024).decode()
if 'Incorrect' not in answer:
    stop = True
    print(answer)
sock.close()
````

Then because this was kind of slow I moved this to `threaded` application which then only took a few seconds. (5 threads)

Full script:
````python
    import socket
    import random

    def find_letter(shifted_mod, number_used, possible_values):
        chars = 'abcdefghijklmnopqrstuvwxyz'.upper()
        temp_possible = []
        for char in chars:
            orded = ord(char)
            if (number_used << 16) % orded == shifted_mod:
                temp_possible.append(char)

        possible = []
        if len(possible_values) == 0:
            return temp_possible
        
        for char in temp_possible:
            if char in possible_values:
                possible.append(char)

        return possible


    def main():
        global stop
        while True:
            sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            sock.connect(("challs.n00bzunit3d.xyz", 51081))
            sock.recv(1024)
            flag = ""
            for i in range(16):
                possible_values = []
                for j in range(3):
                    number_used = j + 1
                    sock.send(f"{number_used}".encode() + b'\n')
                    number = sock.recv(1024).decode().split('\n')[0]
                    print(number)
                    possible_values = find_letter(int(number), j + 1, possible_values)
                if len(possible_values) != 1:
                    flag += possible_values[random.randint(0, len(possible_values) - 1)]
                else:
                    flag += possible_values[0]

            sock.send(flag.encode() + '\n'.encode())
            answer = sock.recv(1024).decode()
            if 'Incorrect' not in answer:
                stop = True
                print(answer)
            sock.close()
            if stop:
                break

    import threading

    stop = False

    if __name__ == "__main__":
        for i in range(5):
            threading.Thread(target=main).start()
````

This script is way over engineered and could be made a lot more efficient, but in CTFs you just need it to work :)

Thank you NoobMaster for this amazing challenge. :)

# FLAG CAPTURED!