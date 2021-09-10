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

Regular expressions always seemed to be pretty daunting, with all those weird characters arranged in a particular way seemed to somehow accomplish complicated tasks such as matching all the varied set of patterns a certain string input could be in or find and replace its substrings which matched a certain pattern. Almost always, working with a deadline looming, my approach was to search for the right regular expression that would fit the need then test it with a varied set of inputs and get on with the rest of the work.

Lately I decided to brush up and a delve a bit deeper into this seemingly complicated and geeky art of conjuring up regular expressions. I tried looking up resources that could help me and then found the ones below

1. https://www.freecodecamp.org/news/learn-regular-expressions-with-this-free-course-37511963d278/
2. https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/
3. https://regexone.com/
4. https://github.com/ziishaned/learn-regex
5. https://www.regular-expressions.info/tutorial.html
6. https://www.youtube.com/watch?v=rhzKDrUiJVk 

After quickly skimming through I found 1, 3, 4, 6 at beginner level & 2, 5 a bit more elaborate of the first set I chose 6 the [youtube video](https://www.youtube.com/watch?v=rhzKDrUiJVk) from [web dev simplified](https://www.youtube.com/channel/UCFbNIlppjAuEX4znoulh0Cw) as it seemed straight to the point from the word go. The video as the title suggests really helped me understand the regular expressions real quick so kudos to the channel. I also went through 3 which helped me further understand and apply my learning.

So let me summarize what i learnt from these sources. I have used the [Regexr](https://regexr.com) site to try out all the concepts explained in the video which is also suggested by the video.

1. Search by literal characters
    when we think of a string it is basically a sequence of characters and with regualar expressions we write patterns which match a certain sequence of characters but in simplest of cases those patterns themselves can be literal characters.

    {{<figure src="images/regex1.png" >}}

Search by literal characters is all good but the real power of regular expressions lies in use of some special characters which give a sort of short hand way of representing a sequence of characters

2. First of such  characters is "+", by suffixing any character with "+" in a regular expression it literally means asking the regex engine to search for 1 or more of the character which immedeately precede the "+" character so an expression "e+" would mean to search for substrings which consist of "at least" 1 "e" as shown below.

    {{<figure src="images/regex2.png" >}}

3. If the special character "+" stood for 1 or more of the preceding character then the "?" stands for 0 or 1 of the preceding character, so adding onto the earlier expression "e+k?" would mean search for a substring which atleast has 1 e followed by a k which is optional so "e", "ee", "eeeeek", "eeeee" are all matches while the string "k" or any other string that doesnt contain an "e" isnt a match, as we can see below
    
    {{<figure src="images/regex3.png" >}}

4. Similar to the "+" character is the "\*" character which matches zero or more of the preceding character, so it actually means the character to be searched is optional but when it occurs it may be repeated any number of times. So an expression "e*" will match a sequence of "e" character

    {{<figure src="images/regex4.png" >}}

5. The "." (dot) character represents the wildcard it can match any single character, any meaning letter, digit, whitespace, everything. so what do we do when we need to find the "." character itself, simple we prefix it with  a "\\." in the below example the expression ".at\.?" matches a string which starts with any "." character and then is followed by "at" and then optionally ends with a literal "." character represented by "\.?" hence the words "fate", "cat", "cat." end up being selected.

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

8. Grouping characters with square brackets "[]" and paranthesis "()". We can search for range of characters with say "[a-z]" which essentialy means match any character between "a" and "z" and we can also match mutliple types of characters inside these groups so "[A-Za-z]" says match any character between "A" and "Z" or "a" and "z"

    {{<figure src="images/regex11.png" >}}

    {{<figure src="images/regex12.png" >}}


    https://gohugo.io/content-management/organization/
    https://gohugo.io/content-management/page-bundles/
    https://gohugo.io/content-management/page-resources/
    https://gohugo.io/content-management/image-processing/

    https://adityatelange.in/blog/hugo-watermarking-images/