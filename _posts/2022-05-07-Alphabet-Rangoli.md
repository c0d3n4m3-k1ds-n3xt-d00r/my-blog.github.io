## Hacker Rank: Alphabet Rangoli (Easy)

One of my favourite problem as it looks really cool when you print it out. Though I think this problem should have higher difficulty than Merge The Tools. 

Basically, you are given an integer N. Your task is to print out an alphabet rangoli size N (You can google what a rangoli is).

This is what it will look like once you printed out:

#size 3
```
----c----
--c-b-c--
c-b-a-b-c
--c-b-c--
----c----
```
#size 5
```
--------e--------
------e-d-e------
----e-d-c-d-e----
--e-d-c-b-c-d-e--
e-d-c-b-a-b-c-d-e
--e-d-c-b-c-d-e--
----e-d-c-d-e----
------e-d-e------
--------e--------
```
Now that I look at it again, I once again have no idea how I solved this a few weeks

Anyway, let's see how my past-self analyzed the problem from given examples:

First of all, you can easily see that the rangoli has two axes of symmetry, vertically and horizontally. Therefore, as soon as I come up with for-loop logic of one halves I can apply the same logic again on the other halves. In other word, instead of solving one whole problem, I just need to technically do 50% of it.

Secondly, I need to find the linear equation between the size of the rangoli and the number of dashes (or how "centered" the alphabetic characters are)

You can stop here if you want figure it out yourself, my hint is to know about .rjust(), .ljust() and .center() methods.

This is what I came up with:

```
import string
def print_rangoli(size):
    # Make a list that contains the alphabet
    alpha = list(string.ascii_lowercase)
    # Slice the alpha list and assign as "used" to get the alphabetic characters that gonna appear
    used = alpha[0:size]
    # Reverse the used list for future need
    used.reverse()
    # Initialize an empty variable named seq short for sequence
    seq = ''
    # Initialize a prev list short for previous
    prev = []
    # A for-loop of the top half of the rangoli
    for i in range(size):
        """The linear equation between size and spaces replaced by dash is 2*size - 2
        The logic is to have a single changing alphabet letter in the middle/center
        works as the midpoint for the seq variable, seq[::-1] is to horizontally
        flip the seq variable to make it looks mirrored"""
        print(seq.rjust((2*size-2),'-') + used[i] + seq[::-1].ljust((2*size-2),'-'))        
        # prev contains all the seq variables in the for-loop for future need
        prev.append(seq)
        # A growing seq: ''(nothing)-, c-, c-b- for example
        seq = seq + used[i] + '-'
    
    """prev and used list then pop the midpoint of the diamond shaped rangoli
    c-b- and a (like in the size = 3 example). After, this two lists reverse
    for the below half of the rangoli, applied the same logic as the top half"""
    prev.pop()
    used.pop()
    prev.reverse()
    used.reverse()
    
    for j in range(size-1):
        print(prev[j].rjust((2*size-2),'-') + used[j] + prev[j][::-1].ljust((2*size-2),'-'))
        
if __name__ == '__main__':
    n = int(input())
    print_rangoli(n)
```

Gotta admit though, it looks ugly, but it works so I was happy with it(I guess?). I also want to challenge you to come up with another way to do this using 
'-'.join(). as it should be better than my program.
