# Regular Expressions in Python

## Additional Resources

- [Regular Expressions in Python (HOW TO)](https://docs.python.org/3/howto/regex.html)
- [Python Regular Expressions (Google for Education)](https://developers.google.com/edu/python/regular-expressions)

## What are Regular Expressions (regex)?
    
- Editing and searching through text files is one of the most common tasks in computational biology.
- Often these files contain datasets whose format needs to be changed or from which some information needs to be extracted.
- Accomplishing these tasks by hand is tedious at best, and impossible in many cases.
- Regular expressions are an advanced form of find-and-replace that can be used to manipulate complex patterns of text.
- Regular expressions are often abbreviated as `regex`.
- Regular expression syntax can be used in a variety of different programs and contexts, including Python, Sublime, `grep`, and `sed`.

## Regex Tutorial Jupyter Notebook

All of the example code that follows can be found in the `regexTutorial` Jupyter notebook in this repository.

## Importing and Using the `re` Module

The `re` module gives us the ability to use regular expression syntax in Python code. To use this functionality, we'll need to start by importing it

`import re`

While there is a lot that we can do using `re`, we'll start by focusing on two very useful tasks: searching for patterns of text and doing find-and-replace.

After importing `re`, you can search for a particular pattern of text in a string using `re.search(r<pattern>,<string>)`

`re.search(r"ttt","aggtttcctttagttt")`

The letter `r` that appears before the search pattern tells Python that you're searching for raw text. You'll pretty much always want to use it, so just get in the habit of having it there. If the pattern is found, the `search` function will return an `re.Match` object. If the pattern is not found, it will return the value `None`.  You can use this to execute an `if...else` statement

```
if re.search(r"ttt","aggtttcctttagttt"):
    print("Pattern found!")
else:
    print("Pattern not found!")
```

The other function from `re` that we'll need to use a lot is `re.sub()`. This substitution function allows us to conduct find-and-replace. The general syntax is `re.sub(<findPattern>,<replacePattern>,<string>)`.

`re.sub("ttt","TTT","aggtttcctttagttt")`

## Basic Pattern Matching

The simplest types of patterns that you can work with using regular expressions are literal strings of characters, as in typical find-and-replace operations you've probably done in a text editor or word processing program. The example we've already used of a precise pattern of nucleotides in a DNA sequence uses the literal string "ttt" as a search pattern.

`re.search(r"ttt","aggtttcctttagttt")`

Literal patterns are very useful in many contexts, but the real power of regex comes from its flexibility. For instance, let's say that we wanted to search for any part of the sequence where there is a g, then any 3 nucleotides, then a c. In this case, we don't care what the intervening 3 nucleotides are, so we can use a wildcard character that could match any of them. The simplest regex wildcard is `.`, which matches any character other than a newline. Now, our search is

`match = re.search(r"g...c","aggtttcctttagttt")`

If we store the output of our regex search in a variable (in this case, called `match`), we can take a look at the actual sequence that matched our pattern by using the `.group()` method of the object that was returned from our search.

`match.group()`

In this case, our search found the pattern "gtttc". However, take a look at the same search, but with a different sequence this time

```
match = re.search(r"g...c","attcgaagcaggtttcct")
match.group()
```

By default, `re.search()` looks for the _leftmost_ match. In other words, it starts at the left and works its way down the string until it finds a match, then it stops. If we want all the matches for our pattern in a string, we can use the `re.findall()` function.

```
allMatches = re.findall(r"g...c","attcgaagcaggtttcct")
print(allMatches)
```

`re.findall()` returns a list of strings, where each element is a separate match from our search.

There are many other wildcard characters beyond `.`.

`\w` - Matches a 'word' character, which includes all letters (both uppercase and lowercase) and numbers
`\s` - Matches a whitespace character, including spaces, tabs, and newlines
`\t` - Matches a tab
`\n` - Matches a newline
`\d` - Matches a number (0-9).
`^` - Matches the beginning of the string
`$` - Matches the end of the string

See the tutorial Jupyter notebook for examples that use these wildcards.

Often, using an uppercase letter in place of a lowercase letter reverses the nature of the wildcard. For instance, `\S` matches any __non__-whitespace character and `\W` matches any __non__-word character.

You can also create any custom wildcard that you want by putting the characters that you want to match inside square brackets. For example, here we're using wildcards to match an even number followed by an odd number. Note that the brackets themselves are __not__ part of the search. 

```
match = re.search(r"[2,4,6,8][1,3,5,7,9]","1249787")
match.group()
```

## Escaping Special Characters

Sometimes you actually want to search for the literal version of a special character. For instance, let's say we're trying to find a dollar sign - `$`. By default, regex will interpret this character as indicating the end of our string. However, we can "escape" the special nature of `$` by adding a `\` before it. Therefore, `\` is often referred to as the "escape character".

```
match = re.search(r"\$\d","I have $1.")
match.group()
```

## Searches with Repetition

Wildcards add a huge amount of flexibility to our searches, but so far we've still had to list as many characters in our pattern as we want to match in our string. What if some characters repeat themselves, perhaps an unknown number of times?

To match one or more instances of a character, we can use `+`. Here, we're looking for a string of As

```
match = re.search(r"A+","GCTTTGGAAAGG")
match.group()
```

To match a specific number of instances of a character, we can put the specific number inside curly braces

```
match = re.search(r"A{2}","GCTTTGGAAAGG")
match.group()
```

To match _zero_ or more instances of a character, we can use `*`. To get a feel for how `*` works, compare the outputs from the following two searches

```
matchOne = re.search(r"CT*G","GCTTTGGAAAGG")
matchOne.group()

matchTwo = re.search(r"GT*G","GCTTTGGAAAGG")
matchTwo.group()
```

Repetition characters can also be used in combination with wildcards.

```
match = re.search(r"C.+G","GCTTTGGAAAGG")
match.group()
```
