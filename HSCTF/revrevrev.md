# rev/revrevrev

## By Jaysu

## Description

Your friend is trying to pass this car game that he made. Sadly, they have long term memory loss and they don't remember the inputs or the goal of the game. All they have is the code. You should help them, as something hidden (like a flag?) will display if you find the correct inputs.

Note: if you have a flag that works on the challenge file but isn't accepted, please DM the author (or another organizer).

Attachments:
[revrevrev.py](https://hsctf-10-resources.storage.googleapis.com/uploads/a95cfe259e3d1119e706d50998e369913ac1044784122a3eb03eee4580077818/revrevrev.py)

## Solution

````python
ins = ""
while len(ins) != 20:
  ins = input("input a string of size 20: ")

s = 0
a = 0
x = 0
y = 0
for c in ins:
  if c == 'r': # rev
    s += 1
  elif c == 'L': # left
    a = (a + 1) % 4
  elif c == 'R': # right
    a = (a + 3) % 4
  else:
    print("this character is not necessary for the solution.")
  if a == 0:
    x += s
  elif a == 1:
    y += s
  elif a == 2:
    x -= s
  elif a == 3:
    y -= s
print((x, y))
if x == 168 and y == 32:
  print("flag{" + ins + "}")
else:
  print("incorrect sadly")
````

## Code explanation

This is a very basic script where the input is the length of 20 and where we only get to choose from 3 letters `r`, `L`, `R`.

This is easily bruteforcable, because we just have to run the loop and see if the output is vector (168, 32)

I have recently taken interest in GoLang and this seemed like a great place to start, because since we are bruteforcing we need a lot of speed. So with the help of ChatGPT and my brain I came up with this abomination:

````go
package main

import "fmt"

func main() {
	stringGenerator := generateString(20)

	for i := 0; i < 200; i++ {
		input := <-stringGenerator
		if isCorrect(input) {
			break
		}
	}
}

var chars = []string{"r", "L", "R"}

func generateString(length int) <-chan string {
	ch := make(chan string)
	go func() {
		defer close(ch)
		generate("", length, ch)
	}()
	return ch
}

func generate(current string, length int, ch chan<- string) {
	if length == 0 {
		ch <- current
		return
	}

	for _, char := range chars {
		generate(current+char, length-1, ch)
	}
}

func isCorrect(input string) bool {
	s := 0
	a := 0
	x := 0
	y := 0

	for _, c := range input {
		if c == 'r' { // rev
			s += 1
		} else if c == 'L' { // left
			a = (a + 1) % 4
		} else if c == 'R' { // right
			a = (a + 3) % 4
		}

		if a == 0 {
			x += s
		} else if a == 1 {
			y += s
		} else if a == 2 {
			x -= s
		} else if a == 3 {
			y -= s
		}
	}

	fmt.Println(x, y)

	if x == 168 && y == 32 {
		fmt.Println("flag{" + input + "}")
		return true
	}

	return false
}
````

ChatGPT was responsible for generating me characters :)

So and compiling and running this only takes no more then 0 seconds to find the right flag, submit it and we get the points!.

````
####>revrevrev.exe
210 0
190 19
190 -19
171 37
153 18
189 18
171 -37
189 -18
153 -18
153 54
135 35
171 35
118 17
136 0
136 34
188 17
170 34
170 0
153 -54
171 -35
135 -35
188 -17
170 0
170 -34
118 -17
136 -34
136 0
136 70
118 51
154 51
101 33
119 16
119 50
171 33
153 50
153 16
85 16
103 -1
103 33
120 -17
136 0
104 0
120 49
104 32
136 32
187 16
169 33
169 -1
152 49
136 32
168 32
flag{rrrrrrrrrrrrrrrrLRLR}
````

Thanks to Jaysu for this awesome challenge

# FLAG CAPTURED!