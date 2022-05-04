## The Minion Game

WARNING
VIEWER DISCRETION IS ADVISED

RAGING INCOMING...

GOD FORSAKEN THIS M.THERFU.CKING QUESTION ON HACKERRANK, WHY THE F.CK IT IS SO CONFUSING, STUPID TEST CASES AND THE GAME

Okay I feel better now, you have been warned anyway.

Umm, care to elaborate? - you probably asked.

Sure, with pleasure.

You must be wondering: what is Hacker Rank? If you are learning to code like me, you should know this website (if not, what have you been doing with your life). For those who don't know, this is a free website that help you learn programming with tons of exercises and practices in different programming languages ranging from C++ to Python. 

Next, why am I mad? Remember when I said programming is annoying? No? Well it is, and it's annoying as fuck, especially when you spent days turning to weeks on this problem and you just can't get past some test cases.

And that problem is called 'The Minion Game.'

```
Basically, in this game, there will be two players named Stuart and Kevin. 

They will be given a 'string' of letters (it's a word but not really one as some are literally 
just "AWWJFNW..." like a teenager slamming their keyboard). 

The players then need to find the subset of the given 'string' of letters (or substring in other word). 

The catch here is that Stuart's substrings have to start with consonants, whereas Kevin's substrings start with vowels.

Whoever finds the most substrings win, if they have same amount of substrings, they are draw.
```
```
For example: 'hello'

Stuart's substrings are: 'h', 'he', 'hel', 'hell', 'hello', 'l', 'll', 'llo', 'l'

Kevin's substrings are: 'e', 'el', 'ell', 'ello', 'o'
```

Obviously Stuart wins here

And that's also what I need to code, I will be given a 'string' of letters, find out who wins the game or if they are draw.
The logic is quite simple here, as you can see from my example above, the 'h' is accumulatingly added with the rest of the letters, then it moves on with the next letter, repeated the loop, and so on until the last letter is 'o'.

It will look like this if you write it down:

```
string = 'hello'
for i in range(len(string)):
   substr = string[i]
   for j in range(i+1, len(string)):
      substr += string[j]
```
About Stuart and Kevin scores here, it's just a simple check whether the first word is consonant or vowels.
So this is my first approach to the problem
```
def minion_game(string):
    vowels = 'UEOAI'
    Kevin, Stuart = 0, 0
    for i in range(len(string)):
        substr = string[i]
        if substr not in vowels:
            player = 'Stuart'
            Stuart += 1
        else:
            player = 'Kevin'
            Kevin += 1
        for j in range(i+1, len(string)):
            substr += string[j]
            if player == 'Stuart':
                Stuart += 1
            else: Kevin += 1
    
    if Stuart > Kevin:
        print(f'Stuart {Stuart}')
    elif Stuart < Kevin:
        print(f'Kevin {Kevin}')
    else:
        print('Draw')
```
There would be nothing to say if I got through all the test cases. As a matter of fact, test cases must be the most confusing part of this problem. They really complicated a simple game like this.

The last test case requires an output of *Kevin 400173457964* within a time limit of under 2 seconds

Imagine loop through a nested for-loop 400173457964 times.

If you are still unable to understand how fucking TERRIBLE this test case is, my first approach probably run this test in 10 mins, well for the record, I haven't tried this test case as I quitted after 2 mins of processing.
My program can't even handle simpler test case of *Stuart 7501500* as a result.
```
NANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANANNANAN...
```
Anyway, this is one of the test case, the NA will just go on and on for like thounsands of letters. 

So what did I do next to increase my program efficiency?

(Continue in part 2)






