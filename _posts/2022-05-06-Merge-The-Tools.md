## HackerRank: Merge The Tools (Medium)

I just wanna say, it feels nice to be distracted and get a problem done in once a while.

I have been so frustrated with the Minion Game and the technical issues with the autograder in this Python learning website that it started to be a bit demotivated for me to finish my daily coding goal.

Anyway, here is the problem, Imma just copy-paste straight away from HackerRank.

Given:

- A string s, of length n where s doesn't have any spaces
- An integer, k, where k is the factor of n

We can split s into n/k substrings where each subtring, t(i), consists of a contiguous block of k characters in s. Then, use each t(i) to create string u(i) such that:

- The characters in u(i) are a subsequence of the characters in t(i).
- Any repeat occurrence of a character is removed from the string such that each character in u(i) occurs exactly once. In other words, if the character at some index j in t(i) occurs at a previous index < j in t(i), then do not include the character in string u(i).

Confusing isn't it? A bit, but you will understand right away with the example below.

Example:

s = 'AAABCADDE'

k = 3

There are three substrings of length  to consider: 'AAA', 'BCA' and 'DDE'. The first substring is all 'A' characters, so u(1) = 'A'. The second substring has all distinct characters, so u(2) = 'BCA'. The third substring has 2 different characters, so u(3) = 'DE'. Note that a subsequence maintains the original order of characters encountered. The order of characters in each subsequence shown is important.

Some initial thoughts about the trouble you may encounter:

- First of all, splitting/slicing string s into (len(string)/k) substrings.
- Secondly, remove the repeat occurences of a charater from the substrings so that each character in u(i) occurs exactly once and maintain the order of characters.

You can stop here and solve it yourself if you want.

Not gonna brag but as soon as I saw and read this prblem carefully, I knew I could be solve it a few hours or less. As it's just straight up testing your problem-solving skill, I don't need to learn anything new like in The Minion Game. However, having tried another Medium level before, the only thing I fear is the test cases as they may contain runtime Error.

I then proceed to analyze the mentioned troubles:

- Firstly, the splitting string can be solved easily with textwrap module using wrap() method, I mean you can slice and loop through the string s but it's too much for me to even give some thoughts (Cuz I did give it a thought).
- Moving on the second trouble, my initial idea was to do something similar to The Minion Game, write a function to find all the repeated charaters, remove their occurences and finally print out the wanted unique character substrings. However, I quickly got rid of that idea as the progam will be very inefficient with all those nested loops (find() and remove()). I then came up with the idea to use dictionary to get all that unique keys at once while maintianing their order.

This is my final work:
```
from textwrap import wrap
def merge_the_tools(string, k):
    sublist = wrap(string, k)
    for sub in sublist:
        subdict = dict()
        for s in sub:
            subdict[s] = subdict.get(s,0) + 1
        print(''.join(list(subdict.keys())))
if __name__ == '__main__':
    string, k = input(), int(input())
    merge_the_tools(string, k)
```


