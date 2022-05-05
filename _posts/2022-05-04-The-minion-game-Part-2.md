## The Minion Game (Part 2)

So what did I do after my first approach failed?

Duh, I studied more of course, you think I would give up? Yeah well not now, not until I have tried everything I can that I look for the solution to this problem.

Anyway, if you know about how time complexity works in Python (Big O Notation), you will see those changes that I came up with for my program made a very insignificant effort in reducing the runtime, not to mention some changes are not even an improvement. However, please bear with me as it is still interesting to see how I went from having 30+ lines of codes to half of it without a nested for-loop.

After analysing what's wrong with my first approach, I decided to reduce as many unnecessary overlapped counts as possible.

For example: 'BANANA'

I will assume you are already familiar with game's rules (if you aren't, go back to the first part of The Minion Game) enough to figure out Stuart's substrings and Kevin's substrings.

Some overlapped counts will be: 'A', 'N', 'NA', 'AN', 'ANA'.

My first program will just count each of the substrings seperately (Stuart += 1 or Kevin += 1), which is quite time-consuming. As a result, I decided to count all the overlapped substrings at once, store it using key: value pair, and finally make a check if their values are already stored.

Yes I'm talking about using dictionaries and a count_overlapping_substring function.

So instead of loop through each substrings, I will have {'A':3, 'N':2, 'NA':2, 'AN':2, 'ANA':2,....} you get the idea. By doing this, I don't need to loop through those keys for multiple times. 

Theoretically, this idea sounds good right? Well, no, not exactly. By doing the count_overlapping_function, I was basically doing another loop to find all the overlapped substrings, an even bigger flaw than my first attempt.

Did I realise this? No, I was so fixated on trying out different ways to count all the substrings that it distracted me from the main problem.

(Continue in Part 3)




