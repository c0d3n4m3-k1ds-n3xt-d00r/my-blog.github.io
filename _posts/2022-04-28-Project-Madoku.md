I think I will never stop bragging about this as it is the first project I have ever come up with myself. 
I mean look at how long I wrote, like...like...to be honest I don't know what to say, just really proud
of myself at that time you know. A whole week - 7 days working on it, think about it nonstop, keep adding
more things to the game until my mind would stop telling me to add more features to the game.

An quick introduction to the project. I called this game Madoku, as in Math Sudoku. I got inspired from 
NERDLE, you can google the game if you don't know about it. There's a small story behind why I got inspired
by it. You see, before this Project, I was planning to do like a Hang Man word game. However, what I don't 
like about Hang Man game is that I have to make an addition file that contains the word, which doesn't have
that freedom that I want. Therefore, as soon as I saw this game on Disguised Toast stream, I got hooked 
rightaway in re-creating it.

Some difficulties that I encountered during the project:
- First of all, about how to make the game has the most freedom as possible, I thought a lot about making the
solution or calculation as random as possible. At first I want to make the spaces fixed in 8 spaces like
NERDLE, but after a while I figured out a way to make possible random solution without any fixed element
- Secondly, I thought a bit how to update the game screen as well, like when you input your guess and the game
got updated on the screen. I didn't know about global variables few days ago, it's truly a game changer when I 
learned about it. 
- Thirdly, the biggest problem, giving the hint of how close user guess to the solution, it looks simple but 
there were few complicated things when I started working on it. My initial thought is like the game NERDLE, 
  If their guesses have numbers that not in the solution, print out those number not in solution.
  If their guesses have numbers that is in the solution, check if it is in correct position or not.
    + So the first problem was to compare the guess and the solution, which I then figured out by comparing the 
    two dictionaries of their guess and the solution, using intersection() of sets.
    + I got the common keys between the guess and solution, which is really useful, after a few debugging
    I remembered that index() and find() only find the first index of the element, which means if there
    are more than 2 same numbers in the common keys, the hint will be wrong. Like imagine this:
    "1+1=2" is the solution
    And "4-2=2" is their guess: In this guess, the hint would initially say 2 is in wrong position, which is false,
    cuz there is one " 2 " in the correct position.
    Or "3-1=2" is their guess: In this guess, the hint would initially say 1 is in wrong position, which is false,
    cuz there are two " 1 " in the solution, and 1 in the guess should be correct position
    + I got quite stressed when I found out this problem cuz I though Im already done with the game. So my approach
    was to find the positions of the common key of both guess and solution, then compare the size of the two 
    position set and also find the intersection() of the two sets to see if there is at least one position is 
    correct. About how to find the position, my approach is like finding a substring, which I have done before.
    So back to the example:
    "1+1=2" is the solution
    And "4-2=2" is their guess, so there are two " 2 " in their guess, the position set size of their guess is bigger 
    than the solution set and their intersection is > 0, the hint then says "There is at least one ' 2 ' in correct 
    position. 
    Or "3-1=2" is their guess, the same principle applied, and the hint says "There is at least one ' 1 ' in correct 
    position, which hints the player/user that there are more than one ' 1 ' in the solution.
    + Of course, if the position set intersection() is < 0, it will hint All ... not in correct position.
    + Having function within function is the first time, I didn't even think it works.
- Last but not least, a bit of thought about the game difficulties, which then solved by creating a global difficulty
dictionaries, I have to add/change parameters of some functions here and there as I didn't plan to make difficulty
at first. I only thought about it after letting my brother played and it was too hard.

Some thoughts after the project:
- It was an absolutely fun experience as I learned tons throughout the project, including global variables, sets methods.
- Although the project works and runs, I am not confident in its time complexity as I think I overused list, like
if it's temporary local variable and I can just create a tuple for better efficiency. However, I only learned about
tuple efficiency after the project so that's a shame.
- I haven't added comments, which is a bad habbit from my side cuz I won't remember anything when I look at the project 
a while later. 



```tsql
import random
import time
```
```
solution = []
gameDict = {}
gameScreen = []
gamemode = {'easy': (8,(0,100),' '), 'normal':(7,(0,500),'+-*/ '), 'hard':(6,(0,1000),'0123456789 ')}
difficulty = ()
countGuess = 0
userNotWin = True
areYouWinningSon = False
correctKey = []
```
```
def test(): # Testing, hide this once complete the project
    print(solution) # Testing the solution
    print(gameDict) # Testing the random dictionary
    print(difficulty) # Testing the dificulty
```
```
def setMode(mode):
    while mode == 'custom':
        try:
            guess = int(input('How many guesses you want?\n\n'))
            lowest = int(input('The lowest limit of the solution (lowest < solution):\n\n'))
            highest = int(input('The highest limit of the solution (solution < highest):\n\n'))
            if lowest > highest:
                raise ValueError('The lowest limit cannot be higher than highest limit, duh')
            elif abs(highest - lowest) < 2:
                raise ValueError("I don't think you want to play with decimal")
        except ValueError as error:
            print(repr(error))
            doAfterExcept()
            continue
        except:
            print('Please enter an integer number')
            doAfterExcept()
            continue
        gamemode[mode] = (guess,(lowest,highest),' ')
        break
    global difficulty
    difficulty = gamemode.get(mode)
```
```
def randomDict(solution):
    g_dict = {}
    for element in solution:
        g_dict[element] = g_dict.get(element, 0) + 1
    global gameDict
    gameDict = g_dict
```
```
def randomGenerator(nPrevious):
    if nPrevious in ['*','/']:
        nCurrent = random.randint(2,9) 
    elif nPrevious in ['+','-']:
        nCurrent = random.randint(1,9)
    elif nPrevious == '1':
        nCurrent = random.choice(random.choice(['0123456789','+-']))
    else:
        nCurrent = random.choice(random.choice(['0123456789','+-*/', difficulty[2]]))
    return nCurrent
```
```
def randomsolution(diff):
    while True:
        n1 = str(random.randint(1,9))
        n2 = randomGenerator(n1)
        n3 = randomGenerator(n2)
        n4 = randomGenerator(n3)
        n5 = randomGenerator(n4)
        n6 = randomGenerator(n5)
        e = "{}{}{}{}{}{}".format(n1,n2,n3,n4,n5,n6)
        e = ''.join([i for i in e if i != ' '])
        try: 
            cal = eval(e)
        except:
            continue
        if cal > diff[0] and cal < diff[1] and str(cal) != e and type(cal) == int:
        # Change the cal limit to reduce the difficulty of the game
            break    
    global solution
    solution = [i for i in e] + ['='] + list(str(cal))
    return solution
```
```
def updateScreen(screen):
    global gameScreen
    gameScreen = screen
```
```
def doAfterExcept():
    time.sleep(2)
    print(sep = '')
```
```
def updateGame(u_Guess, count):
    userGuess = ' '.join(list(u_Guess))
    gameScreen[count] = userGuess
    updateScreen(gameScreen)
    global countGuess
    countGuess = count + 1
```
```
def checkUserInput(u_Guess):
    gameSolution = ''.join(solution)
    if u_Guess == gameSolution:
        print('Congratulations! You win in {} guess(es)'.format(countGuess))
        global areYouWinningSon
        areYouWinningSon = True
        global userNotWin
        userNotWin = False
        return userNotWin
    userDict = {}
    for element in u_Guess:
        userDict[element] = userDict.get(element, 0) + 1        
    gameDict_keys = set(gameDict.keys())
    userDict_keys = set(userDict.keys())
    shared_keys = gameDict_keys.intersection(userDict_keys)
    keysNotIn = userDict_keys - shared_keys
    shared = ',  '.join(sorted(list(shared_keys)))
    not_shared = ',  '.join(sorted(list(keysNotIn)))
    print('-------------------------------------')
    print(shared, ':  IN the solution\n')
    print(not_shared, ':  NOT IN the solution\n')
    print('-------------------------------------')
    for key in shared_keys:
        global correctKey
        if key not in correctKey:
            correctKey.append(key)            
        if userDict.get(key) == 1 and gameDict.get(key) == 1:
            if u_Guess.index(key) == gameSolution.index(key):
                print(key,' is in CORRECT position')
            else:
                print(key, ' is NOT in CORRECT position')                
        else:
            def getPositionSet(string, dictionary, key):
                keyPosition = set()
                count = 0
                while count < dictionary.get(key):
                    pos = string.find(key)
                    keyPosition.add(pos)
                    string = list(string)
                    string[pos] = 'x'
                    string = ''.join(string)
                    count += 1
                return keyPosition            
            setSolutionPos = getPositionSet(gameSolution, gameDict, key)
            setGuessPos = getPositionSet(u_Guess, userDict, key)
            intersectPos = setSolutionPos.intersection(setGuessPos)        
            if len(intersectPos) > 0:
                print("There is at least one ' {} ' in CORRECT position".format(key))                
            else:
                if len(setSolutionPos) < len(setGuessPos):
                    print("All {} are NOT in CORRECT position".format(key))                
                else:
                    print(key,' is NOT in CORRECT position')        
        print(sep='')
```
```
if __name__ == '__main__':      
    print("""
Welcome to Madoku, where we test your basic math skill. Inspired from the NERDLE game.
HOW TO PLAY:
Guess the Madoku in 6 tries. After each guess, the system will let you know
how close your guess was to the solution.

The game will look like this with your guess:

    1 0 + 1 0 = 2 0
    _ _ _ _ _ _ _ _
    _ _ _ _ _ _ _ _
    _ _ _ _ _ _ _ _
    _ _ _ _ _ _ _ _
    _ _ _ _ _ _ _ _
         
- Each guess is a calculation
- You can use 0 1 2 3 4 5 6 7 8 9 + - * / or =
- It must contain one “=”
- It must only have a number to the right of the “=”, not another calculation
- The number to the right of the "=" (solution) will be based on the difficulty of the game
(Easy: solution less than 100; Normal: solution less than 500; Hard: solution less than 1000)
- Standard order of operations applies, so calculate * and / before + and -
eg. 3+2*5=13 not 25!
""")
    input("Once you're ready, press Enter to start the game...\n")    
    while True:
        setDifficulty = True
        while setDifficulty:
            mode = input('''Select the game difficulty:
    - Easy: 8 guesses and 0 < solution < 100 
    - Normal: 7 guesses and 0 < solution < 500
    - Hard: 6 guesses and 0 < solution < 1000
    - Custom: Choose your number of guesses and solution limit\n\n''')
            mode = mode.lower()
            try:
                if mode == 'easy' or mode == 'normal' or mode == 'hard' or mode == 'custom':
                    pass
                else: raise NameError('Please choose a given difficulty')
            except NameError as error:
                print(repr(error))
                doAfterExcept()
                continue
            break        
        setMode(mode)            
        print("Generating random solution and game screen...")
        time.sleep(1)        
        randomsolution(difficulty[1]) # Update the global variable solution
        randomDict(solution) # Make a dictionary of global variable solution        
        #test() # Testing, hide when complete       
        screen = ['_ '*len(solution) for i in range(difficulty[0])]
        updateScreen(screen)         
        print('\n'.join(gameScreen)+ '\n')
        print('Your calculations is in {} spaces, good luck with {} guesses!\n '.format(len(solution), difficulty[0]))        
        while countGuess < difficulty[0] and userNotWin: # countGuess starts at 0 
            checkBadInput = True
            while checkBadInput:
                guess = str(input('''What is your guess?
    Input your guessing calculation in {} spaces (e.g 1+1=2 is 5 spaces): '''.format(len(solution)) + '\n\n'))
                print(sep = '')          
                try:
                    equalIndex = guess.index('=')
                except:
                    print("You miss the '=' sign there")
                    doAfterExcept()
                    continue              
                beforeEqual = ''.join([str(i) for i in guess[:equalIndex] if i != ' '])
                afterEqual = ''.join([str(i) for i in guess[equalIndex+1:] if i != ' '])
                userGuess = beforeEqual + '=' + afterEqual             
                try:
                    u_solution = eval(beforeEqual)
                    if str(u_solution) != afterEqual or len(userGuess) != len(solution):
                        raise ValueError('Please enter a valid calculation in {} spaces'.format(len(solution)))
                except NameError:
                    print('This is not an equation, please enter valid calculation')
                    doAfterExcept()
                    continue
                except ValueError as error:
                    print(repr(error))
                    doAfterExcept()
                    continue                   
                break
            updateGame(userGuess, countGuess)
            print(sep='')
            print('\n'.join(gameScreen) + '\n')
            checkUserInput(userGuess)
            print('-------------------------------------')
            correct = ',  '.join(sorted(correctKey))
            print('Here are correct elements you have so far:  ' + correct + '\n')
            print('-------------------------------------')            
        if areYouWinningSon == False:
            print("You have one job! Let's try again\n\n")
            s = ''.join(solution)
            print("And the solution is: {}\n\n'".format(s))            
        time.sleep(2)
        gameRestart = input('Press any keys to restart the game or NO to quit\n\n')
        print(sep = '')
        if gameRestart.lower() == 'no' or gameRestart.lower() == 'n':
            break
        else:
            def reset():
                global countGuess
                countGuess = 0
                global userNotWin
                userNotWin = True
                global areYouWinningSon
                areYouWinningSon = False
                global correctKey
                correctKey.clear()
            reset()
            ```
        
    
        
    
