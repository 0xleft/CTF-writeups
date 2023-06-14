# osting/try-to-hack-me

## By noob_abhinav

## Description

My friend `brayannoob` gave me a ctf challenge and told me Try to hack me (I took this quite literally as you will see). Author: noob_abhinav

Hint 1: (not present at the time)
Read the challenge name carefully and try to find out what is it pointing to
View Hint

Hint 2: (not present at the time)
Also there is no need to login for this challenge ignore the email , password all you need is username


## Solution

Go to https://instantusername.com/ and search for the username. The thing that instantly popped out to me was the github profile. 

By visiting it looks like we arrived at the correct page. Looking around some thinks againt popped out to me:

README.md
````markdown
<!-- hi -->
<!-- my secret -->
<!-- username : brayan234 -->
````

Closed pr: https://github.com/brayannoob/BrayanResearch/pull/2/commits/9789380a0aabdda451ea064ecd1e69368f4f4caf
````markdown
<!-- ACCOUNT CREDENTIALS -->
<!-- USERNAME - @brayanduarte.noob -->
<!-- PASSWORD - Brayann00b@2023 -->
<!-- USERNAME - @brayan -->
<!-- PASSWORD - abhinav7607 -->
````

Commit history:
https://github.com/brayannoob/BrayanResearch/commit/71c314b011fe0666a7dfc0bca7ba9c57f937841e
````markdown
For support, email brayanduarte.noob@protonmail.com.
````

We have a lot of information about this guy. I took a very unintentional path in this one.

1. We check for any leaked passwords in https://haveibeenpwned.com/, but it seemed the email has not been a part of a breach.

We can try to log in into the email since we got all the info.
`brayanduarte.noob@protonmail.com:abhinav7607`

1. The email reveals NOTHING. I was quite shocked that this was not the ending.

2. I dug deeper into all of this guys accounts but came up empty. (FractalID, Streamlit)

Later on I asked the organizers if this was intentional and they mentioned it's not and that I should look closer at the name of the challenge.
(They would then go on to post 2 hints for everyone telling everyone this was not a part of the challenge.)

How could I not think of this. "Try to hack me" - TryHackMe! That idea crossed my mind but I never actually tested it.

After a while I found this:

GET
``https://tryhackme.com/api/similar-users/brayan234``

````javascript
[{"userId":"6481582fbc050c00425ed8e4","username":"brayan234","avatar":"https://secure.gravatar.com/avatar/c2b1dc9f599250db29a2491c43ce1526.jpg?s=200&d=robohash&r=x"}]
````
It meant the user exists so we can navigate to ``https://tryhackme.com/p/brayan234`` and find the flag there.

Thanks to noob_abhinav for this challenge. It was very messed up :/

# FLAG CAPTURED!