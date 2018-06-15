#Intro to Algorithms


-
##What is an algorithm?

	An algorithm is a set of instructions for accomplishing a task.
    Every piece of code could be called an algorithm
	Some can speed up your code or some solve interesting problems

-
-

##Different types of algorithms

	Searching
	Graph algorithms to calculate the shortest route to your destination
	To write an AI that plays chess

	f(x) = x × 2
		What is f(5)?

`backticks.surround("sample code");`


-
-
#Linear search
	


-
-
##Linear search(continued)
	
...stuff

-
-
##Linear search(continued)
	```// Java code for linearly search x in arr[].  If x 
// is present  then return its  location,  otherwise
// return -1
class LinearSearch
{
    // This function returns index of element x in arr[]
    static int search(int arr[], int n, int x)
    {
        for (int i = 0; i < n; i++)
        {
            // Return the index of the element if the element
            // is found
            if (arr[i] == x)
                return i;
        }
  
        // return -1 if the element is not found
        return -1;
    }
} ```
The time complexity of above algorithm is O(n).

-
-

##Linear search(continued)
	
...even stuff

-
-
#Linear search(continued)
	

Linear search is rarely used practically because other search algorithms such as the binary search algorithm and hash tables allow significantly faster searching comparison to Linear search.

Also See – Binary Search

Please write comments if you find anything incorrect, or you want to share more information about the topic discussed above


-
-
##Binary Search

-
-
##Binary Search (continued)
    Suppose you’re searching for a person in the phone book (what an old-fashioned sentence!). Their name starts with K.
        Would you start at the beginning or would you jump to the middle
        Facebook login. 
            Search username database.
            You name is karlmegeddon
        How would you solve these search problems: binary search

-
-
##Binary Search (continued)
    Binary search is an algorithm; its input is a sorted list of elements (I’ll explain later why it needs to be sorted). If an element you’re looking for is in that list, binary search returns the position where it’s located. Otherwise, binary search returns null
        Looking for companies in a phone book with binary search (or other example)

-
-
##Binary Search (continued)
Here’s an example of how binary search works. I’m thinking of a number between 1 and 100
    You have to try to guess my number in the fewest tries possible. With every guess, I’ll tell you if your guess is too low, too high, or correct.
    Suppose you start guessing like this: 1, 2, 3, 4 .... Here’s how it would go.
    A bad approach to number guessing
    This is simple search (maybe stupid search would be a better term). With each guess, you’re eliminating only one number. If my number was 99, it could take you 99 guesses to get there!

-
-
##Binary Search (continued)
    A better way to search
        With binary search, you guess the middle number and eliminate half the remaining numbers every time
        100 items -> 50 -> 25 -> 13 -> 7 -> 4 -> 2 -> 1
            Eliminate half the numbers everytime
        Imagine a dictionary with 240,000 words
            Simple Search = ? steps
            Binary search = ? steps
-
-
##Array vs List
	
...stuff

-
-
##Array vs List (continued)
	
...more stuff

-
-
##Array vs List (continued)
	
...even more stuff

-
-
##Array vs List (continued)
	
...all the stuff


-
-
#Big O notation



-
-
##Big O notation (continued)
	
... stuff

-
-
##Big O notation (continued)
	

In mathematics, the 'logarithm' is the inverse function to exponentiation.

In other words, the logarithm of a given number x is the exponent to which another fixed number, the base x, must be raised, to produce that number x.

<span class="texhtml">1000 = 10 × 10 × 10 = 10<sup>3</sup></span>

"logarithm to base 10" of 1000 is 3


The logarithm of x to base b is denoted as
<span class="texhtml">log<sub><i>b</i></sub> (<i>x</i>)</span>

Hence, <span class="texhtml">log<sub><i>10</i></sub> (<i>1000</i>) = 3</span>


-
-
##Big O notation (continued)
	
<img src="img/BigOgraph.jpeg">

-
-
##Big O notation (continued)
	
<img src="img/BigONotationSummary.jpg">


-
-
#Big O notation (continued)
	
...all the stuff


-
-
# To be continued...
	
-
-
# New slide using `-\n-\n`

More content and examples coming soon...

```Java
for(Larger code : samples){
  use ? triple : backticks;
}
```
