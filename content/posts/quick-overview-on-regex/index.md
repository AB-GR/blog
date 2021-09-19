---
author: "Abhilash"
title: "Quick overview on regular expressions"
date: "2021-09-05"
description: "Basic regular expressions stuff and some cool resources that will quickly ramp you up on regex"
tags: ["regex"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

Regular expressions always seemed to be pretty daunting, with all those weird characters arranged in a particular way seemed to somehow accomplish complicated tasks such as matching all the varied set of patterns a certain string input could be in or find and replace substrings which matched a certain pattern. Almost always, working with a deadline looming, my approach was to search for the right regular expression that would fit the need then test it with a varied set of inputs and get on with the rest of the work.

Lately I decided to brush up and a delve a bit deeper into this seemingly complicated and geeky art of conjuring up regular expressions. I tried looking up resources that could help me and then found the ones below.

1. https://www.freecodecamp.org/news/learn-regular-expressions-with-this-free-course-37511963d278/
2. https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/
3. https://regexone.com/
4. https://github.com/ziishaned/learn-regex
5. https://www.regular-expressions.info/tutorial.html
6. https://www.youtube.com/watch?v=rhzKDrUiJVk 

After quickly skimming through I found 1, 3, 4, 6 at beginner level & 2, 5 a bit more elaborate. From the first set I chose no 6 the [youtube video](https://www.youtube.com/watch?v=rhzKDrUiJVk) from [web dev simplified](https://www.youtube.com/channel/UCFbNIlppjAuEX4znoulh0Cw) as it seemed straight to the point from the word go. The video as the title suggests really helped me understand the regular expressions real quick so kudos to the channel. I also went through 3 which helped me further understand and apply my learning.

So let me summarize what i learnt from these sources. I have used the [Regexr](https://regexr.com) site to try out all the concepts explained in the video which is also suggested by the video.

1. Search by literal characters
    when we think of a string it is basically a sequence of characters and with regular expressions we write patterns which match a certain sequence of characters but in simplest of cases those patterns themselves can be literal characters.

    {{<figure src="images/regex1.png" >}}

Search by literal characters is all good but the real power of regular expressions lies in use of some special characters which give a sort of short hand way or formula of representing a certain sequence of characters which conform to certain rules that are dictated by the expression.

2. First of such  characters is "+", by suffixing any character with "+" in a regular expression it means asking the regex engine to search for 1 or more of the character which immedeately precede the "+" character so an expression "e+" would mean to search for substrings which consist of "at least" 1 "e" as shown below.

    {{<figure src="images/regex2.png" >}}

3. If the special character "+" stood for 1 or more of the preceding character then the "?" stands for 0 or 1 of the preceding character, so adding onto the earlier expression "e+k?" would mean search for a substring which atleast has 1 e followed by a k which is optional so "e", "ee", "eeeeek", "eeeee" are all correct matches while the string "k" or any other string that doesnt contain an "e" isnt a match, as we can see below
    
    {{<figure src="images/regex3.png" >}}

    The real power is "\*" can be seen when applied to the earlier example instead of "e+k?" if we try "e\*k?" then the list of matches include ("e", "ee", "eek", "k"), notice "k" is a match because e is an optional character here.

    {{<figure src="images/regex3-1.png" >}}

4. Similar to the "+" character is the "\*" character which matches zero or more of the preceding character, so it actually means the character to be searched is optional but when it occurs it may be repeated any number of times. So an expression "e*" will match a sequence of "e" character.

    {{<figure src="images/regex4.png" >}}

5. The "." (dot) character represents the wildcard it can match any single character, here "any" means letter, digit, whitespace, everything ! so what do we do when we need to find the "." character itself, simple, we prefix it with  a "\\." in the below example the expression ".at\\.?" matches a string which starts with any character and then is followed by "at" and then optionally ends with a literal "." character represented by "\\.?" hence the words "fat", "cat", "cat." end up being selected.

    {{<figure src="images/regex5.png" >}}

6. Next is "\w" which matches any alphanumeric character while there is "\s" which matches any whitespace character then there are opposite versions of these where "\W" is match any non alphanumeric character & "\S" which is match any non whitespace character. The expression "\w\s" matches any alphanumeric followed by a whitespace character

    {{<figure src="images/regex6.png" >}}

    and the expression "\W\S" matches any non alphanumeric, which means whitespace in the below sample, followed by non whitespace which is alphanumeric in here so the exact opposite result when compared to above i.e the whitespace is trailed by a alphanumeric

    {{<figure src="images/regex7.png" >}}

7. All the above special characters match single characters what if we need to match more than 1 character say for example all words more than 4 characters "\w{4,}"

    {{<figure src="images/regex8.png" >}}

    how about matching exactly 3 characters with "\w{3}"

    {{<figure src="images/regex9.png" >}}

    also match between 3 to 4 charactes with "\w{3,4}" which selects all the words in the sentence

    {{<figure src="images/regex10.png" >}}

8. Grouping characters with square brackets "[]" and paranthesis "()". We can match for range of characters with say "[a-z]" which essentialy means match any character between "a" and "z" and we can also match mutliple types of characters inside these groups so "[A-Za-z]" says match any character between "A" and "Z" or "a" and "z"

    {{<figure src="images/regex11.png" >}}

    {{<figure src="images/regex12.png" >}}

    Similarly using the special parentheses "(" and ")" also works towards grouping characters with the difference that the group so formed can be captured or extracted as a separate match. In practice, this can be used to extract information like phone numbers or emails from all sorts of data.

    
    {{<figure src="images/regex14.png" >}}

    The above expression matches a substring of length 2 to 3 which can contain a sequence of t,e,r in any order and can repeat hence the matches er, tre, et.

9. The last set of special characters are caret "^" and "$". The "^" is match the character or group in the suffix only at the beginning of the test string, 

    As seen below the expression tries to match the first T or t at the beginning of our test string

    {{<figure src="images/regex15.png" >}}

    notice it doesnt match the character at the beginning this can be done by adding the multiline flag

    {{<figure src="images/regex16.png" >}}

    opposite is the behaviour of the "$" character it matches regex string to the end of a given test string, below are both non-multiline and multiline match versions of the expression

    {{<figure src="images/regex17.png" >}}

    {{<figure src="images/regex18.png" >}}

This is pretty much all the basic special characters in regular expressions, other than these something of significance which you would probably be needing when you work with regexes are look aheads and look behinds

10. look behind comes in two flavors the positive look behind and the negative look behind. The idea of a positive look behind is to match a character or characters after a certain sequence or expression. For instance lets try to match any character after the word "The" or "the" the positive look behind is indicated by "=" in the sequence "(?<=(T|t)he)."

{{<figure src="images/regex20.png" >}}

Negative look behind is the exact opposite of positive, so with the above example it can be negated with the expression "(?<!(T|t)he)." which basically translates into all characters which are not after "The" or "the". As we can see the result includes all characters except the ones we found with the positive look behind i.e the charcter that followed "(T|t)he".

{{<figure src="images/regex21.png" >}}

11. Again look ahead comes in two flavors the positive and negative look ahead, look ahead matches any character or characters that come before certain character(s) mentioned in the expresion syntax wise its pretty similar to look behind except for the "<" character.

so if we want any character that comes before "at" this would be the expression ".(?=at)"

{{<figure src="images/regex22.png" >}}

and negative of this is an expression ".(?!at)" which translates to all characters that do not come before "at" which basically includes everything excluded in the above result so its a contrast of the above result.

{{<figure src="images/regex23.png" >}}

So thats a really good summary of regular expressions in a nutshell and should immedeately help your work with regex. The links above talk more about regexes in terms of its application and has various working examples for practice. So that is good pointer if you need to dive deeper.