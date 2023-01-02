# Regex_Tutorial

## Summary

This repo is meant to provide the reader with information and tutorials on specific regular express, or regex, functions by breaking down each part of the expression and describing what it does. 

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors

Anchors have special meaning in regular expressions. They do not match any character. Instead, they match a position before or after characters:

```^``` – The caret anchor matches the beginning of the text.  
```$``` – The dollar anchor matches the end of the text.  

### Quantifiers

Quantifiers specify how many instances of a character, group, or character class must be present in the input for a match to be found.  

The quantities ```n``` and ```m``` are integer constants.  
Ordinarily, quantifiers are greedy.  
They cause the regular expression engine to match as many occurrences of particular patterns as possible.  
Appending the ```?``` character to a quantifier makes it lazy.  
It causes the regular expression engine to match as few occurrences as possible.

### OR Operator

Match one of multiple patterns ```|```  
To match one of multiple patterns—such as this OR that—use the OR operator ```|```. For example:  

Example	Matches  
```hi|hello```	A string that contains either hi or hello  
```(b|cd)ef```	A string that contains either bef or cdef  
```(a|b)*c```	A string that has a sequence of alternating as and bs, ending with c  

### Character Classes  

Character classes distinguish kinds od characters such as, for example, distinguising between letters and digits.    

#### Example:     

Character Class - Matches any one of the enclosed characters  
```
[xyz] // matches to with x, y, and z
[a-c] // matches to a, b, and c
```

Negated Or Complemented Character Class - Matches anything that is NOT eclosed in brackets  
```
[^xyz] // matches to anything except x, y, and z
[^a-c] // matches to anything except a, b, and c
```

Dot - Matches any single character except line terminators: ``` \n ```, ```\r```, ```\u2028```, or ```\u2029```    
```
.
```

### Flags

Regular expressions have optional flags that allow for functionality like global searching and case-insensitive searching. These flags can be used separately or together in any order, and are included as part of the regular expression.  

```d```  Generate indices for substring matches.  
```g```  Global search.  
```i```  Case-insensitive search.  
```m```  Allows ^ and $ to match newline characters.  
```s```  Allows . to match newline characters.  
```u```  "Unicode"; treat a pattern as a sequence of Unicode code points.  
```y```  Perform a "sticky" search that matches starting at the current position in the target string.  

### Grouping and Capturing  

Groups group multiple patterns as a whole, and capturing groups provide extra submatch information when using a regular expression pattern to match against a string.

#### Capturing group:  

```
x
```

Matches x and remembers the match. For example, /(foo)/ matches and remembers "foo" in "foo bar".  

#### Named capturing group: 

```
(?<Name>x)
```
Matches "x" and stores it on the groups property of the returned matches under the name specified by <Name>. The angle brackets (< and >) are required for group name.

For example, to extract the United States area code from a phone number, we could use /\((?<area>\d\d\d)\)/. The resulting number would appear under matches.groups.area.  

#### Non-capturing group:  

```
(?:x)
```
Matches "x" but does not remember the match. The matched substring cannot be recalled from the resulting array's elements ([1], …, [n]) or from the predefined RegExp object's properties ($1, …, $9).


### Bracket Expressions

Brackets indicate a set of characters to match. Any individual character between the brackets will match, and you can also use a hyphen to define a set.  

```'elephant'.match(/[abcd]/)``` // -> matches 'a'  

You can use the ^ metacharacter to negate what is between the brackets.  

```'donkey'.match(/[^abcd]/)``` // -> matches 'o'  

### Greedy and Lazy Match  

#### Greedy: 

As Many As Possible (longest match)  

By default, a quantifier tells the engine to match as many instances of its quantified token or subpattern as possible. This behavior is called greedy.    

For instance, take the ```+``` quantifier. It allows the engine to match one or more of the token it quantifies: ```\d+``` can therefore match one or more digits. But "one or more" is rather vague: in the string 123, "one or more digits" (starting from the left) could be 1, 12 or 123. Which of these does ```\d+``` match?  

Because by default quantifiers are greedy, ```\d+``` matches as many digits as possible, i.e. 123. For the match attempt that starts at a given position, a greedy quantifier gives you the longest match.  

#### Lazy: 

As Few As Possible (shortest match)  

In contrast to the standard greedy quantifier, which eats up as many instances of the quantified token as possible, a lazy (sometimes called reluctant) quantifier tells the engine to match as few of the quantified tokens as needed.  
As you'll see in the table below, a regular quantifier is made lazy by appending a ```?``` question mark to it.

Since the ```*``` quantifier allows the engine to match zero or more characters, ```\w*?E``` tells the engine to match zero or more word characters, but as few as needed—which might be none at all—then to match an ```E```. In the string ```123EEE```, starting from the very left, "zero or more word characters then an E" could be ```123E```, ```123EE``` or ```123EEE```. Which of these does ```\w*?E``` match?

Because the ```*?``` quantifier is lazy, ```\w*?``` matches as few characters as possible to allow the overall match attempt to succeed, i.e. 123—and the overall match is ```123E```. For the match attempt that starts at a given position, a lazy quantifier gives you the shortest match.  

### Boundaries

#### Word Boundaries:

```
\b
```
The word boundary ```\b``` matches positions where one side is a word character (usually a letter, digit or underscore  

The regex ```\bcat\b``` would therefore match cat in a black cat, but it wouldn't match it in catatonic, tomcat or certificate.  
Removing one of the boundaries, ```\bcat``` would match cat in catfish, and ```cat\b``` would match cat in tomcat, but not vice-versa.  
Both, of course, would match cat on its own.

#### Non-Word Boundaries:

```
\B
```
```\B``` matches all positions where ```\b``` doesn't match. Therefore, it matches:

When neither side is a word character, for instance at any position in the string $=(@-%++) (including the beginning and end of the string).  
When both sides are a word character, for instance between the H and the i in Hi!

This may not seem very useful, but sometimes ```\B``` is just what you want. For instance,

```\Bcat\B``` will find cat fully surrounded by word characters, as in certificate, but neither on its own nor at the beginning or end of words.  
```cat\B``` will find cat both in certificate and catfish, but neither in tomcat nor on its own.  
```\Bcat``` will find cat both in certificate and tomcat, but neither in catfish nor on its own.  
```\Bcat|cat\B``` will find cat in embedded situation, e.g. in certificate, catfish or tomcat, but not on its own.  

### Back-references  

```
\n
```
Where "n" is a positive integer. A back reference to the last substring matching the n parenthetical in the regular expression (counting left parentheses). For example, /apple(,)\sorange\1/ matches "apple, orange," in "apple, orange, cherry, peach".

```
\k<Name>
```

A back reference to the last substring matching the Named capture group specified by ```<Name>```.

For example, ```/(?<title>\w+)```, yes ```\k<title>/``` matches "Sir, yes Sir" in "Do you copy? Sir, yes Sir!".

### Look-ahead and Look-behind

```(?=foo)```  
Lookahead: Asserts that what immediately follows the current position in the string is foo  

```(?<=foo)```  
Lookbehind:	Asserts that what immediately precedes the current position in the string is foo  

```(?!foo)```  
Negative Lookahead:	Asserts that what immediately follows the current position in the string is not foo  

```(?<!foo)```  
Negative Lookbehind: Asserts that what immediately precedes the current position in the string is not foo  


### Author

[Andrew LaBorde](https://github.com/AndyLaBorde) created this REGEX Tutorial for personal use. 

