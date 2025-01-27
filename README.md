# Intro to Text Manipulation in R

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Overview

Many data sets contain character strings. For example, your data might include tweets from Twitter (which are basically just strings of characters), and perhaps you want to search for occurrences of a certain word or twitter handle. Alternatively, your data might have a location variable that includes city and state abbreviations, and you might want to extract those observations with location containing “NY.”

In this tutorial, you will learn how to manipulate text data using the package stringr and how to match patterns using regular expressions.

[Link to lab overview video in Panopto (ND users only)](https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=1f515207-8f81-4ccd-9ca9-ac9a013f668f)

## Acknowledgements

This lab procedure is adapted from and based on Ryan Miller's ["Intro to Text Manipulation in R"](https://remiller1450.github.io/s230f19/stringr.html) (Fall 2019, Intro to Data Science STA 230 course, Grinnell College).

# Table of Contents

- [Data and Environment Setup](#data-and-environment-setup)
- [Basic `stringr` Syntax](#basic-stringr-syntax)
  * [Extracting and Locating Substrings](#extracting-and-locating-substrings)
    * [`str_sub`](#str_sub)
    * [`str_detect`](#str_detect)
    * [`str_locate`](#str_locate)
    * [`str_locate_all`](#str_locate_all)
  * [Regular Expressions and Metacharacters](#regular-expressions-and-metacharacters)
- [U.S. Phone Number Example](#us-phone-number-example)
- [Matching Brackets or `html` Tags](#matching-brackets-or-html-tags)
- [Optional: Email Validation](#optional-email-validation)
- [Additional Resources](#additional-resources)
- [Lab Notebook Questions](#lab-notebook-questions)

[Click here](https://raw.githubusercontent.com/kwaldenphd/text-manipulation-r/main/text-manipulation-r-markdown.Rmd) and select the "Save as" option to download this lab as an RMarkdown file.

# Data and Environment Setup

1. This lab focuses on the tidyverse `stringr` package.

2. [`stringr`](https://stringr.tidyverse.org/) "provide[s] a cohesive set of functions designed to make working with strings as easy as possible"


```R
# install.packages("stringr") # for processing character strings

library(stringr)
```

3. `stringr` is part of what is known as "the Tidyverse," in the RStudio user community.

4. According to https://www.tidyverse.org/, "the tidyverse is an opinionated collection of R packages designed for data science. All packages share an underlying design philosophy, grammar, and data structures."

5. To install and load all packages that are part of the tidyverse core, you can install the parent `tidyverse` package.

```R
install.package("tidyverse")
library(tidyverse)
```

6. As of December 2020, the tidyverse core incldues the following packages: ggplot2, dplyr, tidyr, readr, purrr, tibble, stringr, forcats.

# Basic `stringr` Syntax

7. `stringr` functions start with `str_` and take a vector of strings as the first argument.

8. The `stringr` commands we'll be working with in this lab include:

Command | Description
--- | ---
`str_sub` | Extract substring from a given start to end position
`str_detect` | Detect presence or absence of first occurance of a substring
`str_locate` | Give position (start, end) of first occurance of a substring
`str_locage_all` | Give positions of all occurrences of a substring
`str_replace` | Replace one substring with another

## Extracting and Locating Substrings

9. We begin by introducing some basic functions in the `stringr` package.

### `str_sub`

10. The `str_sub` function extracts substrings from a string (a string being a sequence of alpha-numeric characters) given the starting and ending position. 

11. This example extracts the characters in the second through fourth position for each string in fruits:

```R
# install.packages("stringr")

library(stringr)
fruits <- c("apple", "pineapple", "Pear", "orange", "peach", "banana")
str_sub(fruits, 2, 4)
```

### `str_detect`

12. The `str_detect` function checks to see if any instance of a pattern occurs in a string.

```R
str_detect(fruits, "p")  #any occurrence of 'p'?
```

13. Note that pattern matching is case-sensitive.

### `str_locate`

14. To locate the position of a pattern within a string, use str_locate:

```R
str_locate(fruits, "an")
```

15. Only the fourth and sixth fruits contain “an”. In the case of “banana,” note that only the first occurrence of “an” is returned.

16. To find all instances of “an” within each string:

```R
str_locate_all(fruits,"an")
```

### `str_locate_all`

17. The command str_locate_all returns a list, or an object where each element is another object (possibly of different types). 

18. In this example our output is a list with six elements, where each element is a matrix with columns start and end.

19. A few other examples:

```R
out <- str_locate_all(fruits, "an")
data.class(out)
```

```R
data.class(out[[1]])
```

```R
out[[6]] # 6th element of the list, corresponding to banana
```

```R
unlist(out) # coerces the list into a vector
```

```R
length(unlist(out))/2    #total number of times "an" occurs in vector fruits
```

## Regular Expressions and Metacharacters

20. Now suppose we want to detect or locate words that begin with “p” or end in “e,” or match a more complex criteria. 

21. A regular expression is a sequence of characters that define a pattern.

22. Let’s detect strings that begin with either “p” or “P”. 

23. The metacharacter `^` is used to indicate the beginning of the string, and `[Pp]` is used to indicate “P” or “p”.

```R
str_detect(fruits, "^[Pp]")
```

```R
str_detect(fruits, "[Pp]") ## Notice the impact of the metacharacter "^"
```

24. Similarly, the metacharacter `$` is used to signify the end of a string.

```R
str_detect(fruits, "e$" )   #end in 'e'
```

25. Other metacharacters that interact with `stringr` package in RStudio:

Character | Description
--- | ---
`.` | Any single character execept `\n`
`*` | Matches at least 0 times
`+` | Matches at least 1 time
`?` | Matches at most 1 time
`$` | Anchors at the end of the string
`^` | Anchors at the beginning of the string
`{n}` | Matches exactly n times
`{n,}` | Matches at least n times
`{,n}` | Matches at most n times
`[]` | List of permitted characters 

26. For more on regular expressions in RStudio: [Regular Expressions Cheat Sheet](https://rstudio.com/wp-content/uploads/2016/09/RegExCheatsheet.pdf)

27. For instance, `.` matches any single character.

28. `gr.y` matches gray, grey, gr9y, grEy, etc.

29. And `*` indicates 0 or more instances of the preceding character.

30. `xy*z` matches xz, xyz, xyyz, xyyyz, xyyyyz, etc.

31. To detect the letter “a” followed by 0 or more occurrences of “p”:

```R
str_detect(fruits, "ap*")     
```

32. Compare the previous command to:
```R
str_detect(fruits, "ap+")
```

33. The `+` in front of the `p` indicates that we want 1 or more occurrences of “p.”

34. Here is a more complex pattern:

```R
str_detect(fruits, "^a(.*)e$") 
```

35. The anchors `^` and `$` are used to indicate we want strings that begin with the letter a and end with e. 

36. The `(.*)` indicates that we want to match 0 or more occurrences of any character. 

37. Parentheses can be used to group parts of the pattern for readability, you’ll get the same results without them.

# U.S. Phone Number Example

38. Suppose we want to extract 10 digit United States phone numbers from a data set of text strings.

```R
a1 <- "Home: 507-645-5489"
a2 <- "Cell: 219.917.9871"
a3 <- "My work phone is 507-202-2332"
a4 <- "I don't have a phone"
info <- c(a1, a2, a3, a4)
info
```

39. To derive a regular expression for valid 10 digit phone numbers we must recognize a few patterns.

40. A United States area code must start with a 2 or higher so we use brackets again to indicate a range: `[2-9]`

41. The next two digits in the area code can be between 0 and 9, so we write `[0-9]{2}`.

42. The area code is separated from the other digits using either a “.” or “-”, so we use `[-.]` to indicate either a dash or a period.

43. The complete regular expression is given below:

```R
phone <- "([2-9][0-9]{2})[-.]([0-9]{3})[-.]([0-9]{4})"
out <- str_detect(info, phone)
out
```

44. Again, `str_detect` just indicates the presence or absence of the pattern in question.

```R
str_extract(info, phone)
```

45. We can also use `stringr` to do make manipulations such as anonymizing the phone numbers:

```R
str_replace(info, phone, "XXX-XXX-XXXX")
```

46. As we noted above, certain characters are reserved. 

47. If we want to actually reference them in a regular expression, either put them within a bracket `[]`, or use a double forward slash `\\`.

```R
str_locate(info, "[.]")  #find first instance of period
```

```R
str_locate(info, "\\.")  #same
```

```R
str_locate(info, ".")    #first instance of any character
```

48. Metacharacters have different meanings within brackets.

```R
str_detect(fruits, "^[Pp]")  #starts with 'P' or 'p'
```

```R
str_detect(fruits, "[^Pp]")  #any character except 'P' or 'p'
```

```R
str_detect(fruits, "^[^Pp]") #start with any character except 'P' or 'p'
```

49. See the [stringr cheatsheet](http://edrub.in/CheatSheets/cheatSheetStringr.pdf) for a summary of regular expressions.

# Matching Brackets or `html` Tags

50. In many cases, you may want to match brackets such as `[8]` or html tags such as `<table>`.

```R
out <- c("abc[8]", "abc[9][20]", "abc[9]def[10][7]", "abc[]")
out
```

51. In order to better understand what regular expressions are matching here, we will replace pieces of the above strings with the character “X”.

52. To replace the left bracket, we write `\\[`. 

53. Next we want to match 0 or more occurrences of any character except the right bracket so we need `[^]]*`. 

54. Finally, to match the right bracket `\\]`.

```R
str_replace_all(out, "\\[([^]]*)\\]", "X")
```

55. Compare the previous example with:

```R
str_replace_all(out, "\\[(.*)\\]", "X")
```

56. In this case, we match the first left bracket (indicated by the `\\[`), followed by 0 or more instances of any character (the `(.*)` portion), which could be a right bracket until the final right bracket `\\]`.

# Optional: Email Validation

57. One real world application of string matching is detecting whether or not an email addres is valid. Examples of some valid email addresses are shown below:
- “simple@example.com”
- “johnsmith@email.gov”
- “marie.curie@college.edu”,
- “very_common@example.com”,
- “a.little.lengthy.but.ok@dept.example.com”,

58. For this question, write code that takes the vector given below and identifies the first 5 email addresses as valid, and indentifies the last 3 as invalid.

59. You may assume that a valid email address has a top-level domain (ie: .com, .gov, .fr, …) that is either 2 or 3 letters. 

60. You may also assume that letters, numbers, `.`, `-`, and `_` are the only valid characters in the core components of the address (see the 6th email below for an invalid email).

```R
emails = c(
"simple@example.com",
"johnsmith@email.gov",
"marie.curie@college.edu",
"very_common@example.com",
"a.little.lengthy.but.ok@dept.example.com",
"bad.email.because+symbol@example.com",
"not_good@email.address",
"this.email.is.fake@gmail.xcom")
```

# Additional Resources

61. Additional resources for using the `stringr` package:
- [`stringr` cheat sheet](http://edrub.in/CheatSheets/cheatSheetStringr.pdf_
- [Chapter 14 "Strings"](https://r4ds.had.co.nz/strings.html) in Hadley Wickham and Garrett Grolemund's [*R for Data Science*](https://r4ds.had.co.nz/index.html) (O'Reilly, 2017).
- Gaston Sanchez, [*Handling and Processing Strings in R*](http://www.gastonsanchez.com/Handling and Processing Strings in R.pdf) (Trowchez Editions, 2013).

# Lab Notebook Questions

## Question 1

```R
veggies <- c("carrot", "bean", "peas", "cabbage", "scallion", "asparagus")
```

Using the vector veggies defined above using stringr functions to do the following:
i. Find those strings that contain the pattern “ea”.
ii. Find those strings that end in “s”.
iii. Find those strings that contain at least two “a”’s.
iv. Find those strings that begin with any letter except “c”.
v. Find the starting and ending position of the pattern “ca” in each string.

## Question 2

The regular expression `"^[Ss](.*)(t+)(.+)(t+)"` matches “scuttlebutt”, “Stetson”, and “Scattter”, but not “Scatter.” Why?
