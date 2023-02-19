# Solving Wordle with grep

This is a series of articles of my learnings from the book The Linux Command Line.

In this article, I try to apply some learnings from the book

Let's try to solve Wordle. 

If you don't know about this game, do a google search, there is an Easter egg also on the search page

![Gif description](https://9to5google.com/wp-content/uploads/sites/4/2022/01/google-wordle.gif)


## grep

global regular expression print(grep) -  it Print Lines Matching a Pattern


grep [options] regex [file...]


Regular expression metacharacters consist of the following:
**^ $ . [ ] { } - ? * + ( ) | \ **

**Anchors** 

The caret (^) and dollar sign ($) characters are treated as anchors in regular expressions.
This means they cause the match to occur only if the regular expression is found at the
beginning of the line (^) or at the end of the line ($)

**Bracket Expressions and Character Classes**

```
grep  '^[ABCDEFGHIJKLMNOPQRSTUVWXZY]' file.txt -- searches lines starting with A-Z

grep  '^[A-Z]' file.txt -- same as above 

grep  '^[A-Za-z0-9]' file.txt -- searches lines starting with alphanumeric character
```

Negation
If the first character in a bracket expression is a caret (^), the remaining characters are
taken to be a set of characters that must not be present at the given character position.


Did you know that your Linux system contains a dictionary? It does. Take a look
in the /usr/share/dict

Let's apply the learnings!!

Game starts

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648105696524/1FpRZCDxm.png)

We have to enter a 5 character word, let's use grep and dictionary to search for some 5 letter words 

```
cd /usr/share/dict
$ grep ^[a-z][a-z][a-z][a-z][a-z]$ american-english  | less
```

there are 4581 words

Let's enter some random a word, I entered "WOMEN"


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648105663060/X516e-DxO.png)

We got that the word contains "e" and it is not in the 4th place, and there is no "w", "o", "m" and "n"

Let's run grep again, giving this input

```
grep ^[a-z][a-z][a-z][a-z][a-z]$ american-english | grep -v w | grep -v o | grep -v m | grep -v n | grep e
```
now we are saying, give me all the 5 character words, and remove all the words that have "w", "o", "m" and "n" and only show the words that have "e"

Now we get a list of just 1135 words ( we reduced the search space by 4 times -- better than binary search)

Let's enter one random word from the list,  I choose "shear"

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648106360276/32AJuCo1C.png)

Now we got to know, that letter "h" is in 2nd place, and the letter "e" is in 3rd place, and there is no "a" and "r"

Let's put this info in grep 

```
grep ^[a-z]he[a-z][a-z]$ american-english | grep -v w | grep -v o | grep -v m | grep -v n | grep -v a | grep -v r  | grep e
```
now we are saying, give me all the 5 character words, that have "h" at 2nd place and "e" at 3rd place, and remove all the words that have "w", "o", "m", "n", "a", and "r" 

Now we get just 15 results (we reduced the search space by 75 times)

Let's enter one random word from the list,  I choose "chess"


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648106599809/PRSk_JtDm.png)

Now we got to know, "ches" are at the correct place, only last character is missing

```
grep ^ches[a-z]$ american-english | grep -v w | grep -v o | grep -v m | grep -v n | grep -v a | grep -v r | grep e
```
now we get only two words chess and chest

**And BINGO, we have solved the puzzle.**

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648106815826/SQ-_QXqNq.png)

I know, solving wordle with dictionary and grep is cheating, but this is just for learning the power of regular expressions with grep :)

For part 1 check this [link](https://til.hashnode.dev/learnings-from-the-book-the-linux-command-line)

You can download the book from [here](https://linuxcommand.org/tlcl.php) 

My this year's target is two read 3 books each month. I share my progress each month on Twiter. You can follow me, and learn something new with me.

%[https://twitter.com/nkalra0123/status/1488765699417784320]

 