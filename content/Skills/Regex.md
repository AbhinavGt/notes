- Regular Expressions, abbreviated as Regex or Regexp, are a string of characters created within the framework of Regex syntax rules.
- You can easily manage your data with Regex
- It uses commands like finding, matching, and editing. 
- Regex can be used in programming languages such as Python, SQL, JavaScript, R, Google Analytics, Google Data Studio
- `/REGEX/gm`, `/REGEX/g`  . For notes regex is between the slash `/` and the text which will get highlighted after regex is applied will be inside backticks symbol $`text`$

 Let's say you have a list of filenames. And you only want to find files with the pdf extension
 ```
readme.md
document.pdf
image.png
music.mp4
manual.pdf

/^\w+\.pdf$/gm
 ```

# Basic
## Basic Matchers
- The character or word we want to find is written directly. It is similar to a normal search process

to find the word curious in the text
```
“I have no special talents. I am only passionately `curious`.”

― Albert Einstein

/curious/gm
```

## Dot (.) : Any Character
- The period `.` allows selecting any individual character, including special characters and spaces

Type a period `.` to mach any character
```
abcABC123.:!?
`a`b`c`A`B`C`1`2`3`.`:`!`?`

/./g
```
# Character Set
## Character Sets `[abc]`
- If one of the characters in a word can be various characters, we write it in square brackets `[]` with all alternative characters. 

To write an expression that can find all the words in the text, type the characters a, e, i, o, u adjacently within square brackets `[]`.
```
bar ber bir bor bur

`bar` ber bir bor bur
/b[a]r/g

`bar` `ber` bir bor bur
/b[ae]r/g

`bar` `ber` `bir` `bor` `bur`
/b[aeiou]r/g

beer deer feer
`beer` `deer` `feer`
/[bdf]eer/g
```

## Negated Character Sets `[^abc]`
- To find all words in the text, except for characters in set
```
bar ber bir bor bur

`bar` `ber` `bir` `bor` `bur`
/b[^]r/g

bar `ber` `bir` `bor` `bur`
/b[^a]r/g

`bar` ber `bir` bor `bur`
/b[^eo]r/g

bear beor beer beur
`bear` beor `beer` beur
/be[^ou]r/g
```

## Letter Range `[a-z]`
- To find the letters in the specified range
- The starting letter and the ending letter are written in square brackets `[]` with a dash between them `-`. 
- It is case-sensitive

Select all lowercase letters between e and o, including themselves.
```
abcdefghijklmnopqrstuvwxyz

abcd`e`f`g`h`i`j`k`l`m`n`o`pqrstuvwxyz
/[e-o]/g
```

## Number Range `[0-9]`
- To find the numbers in the specified range, 
- The starting number and the ending number are written in square brackets `[]` with a dash `-` between them. 

Select all numbers between 3 and 6, including themselves.
```
0123456789

012`3`4`5`6`789
/[3-6]/g
```

# Repetitions
- Some special characters are used to specify how many times a character will be repeated in the text. 
- These special characters are the plus `+`, the asterisk `*`, and the question mark `?`
## Asterisk `*`
- We put an asterisk `*` after a character to indicate that the character may either not match at all or can match many times.

indicate that the letter e should never occur in the text, or it can occur once or more side by side.
```
br ber beer

`br` `ber` `beer`
/be*r/g
```

## Plus Sign `+`
- To indicate that a character can occur one or more times, we put a plus sign + after a character.
 
 indicate that the letter e can occur one or more times in the text.
 ```
 br ber beer
 
 br `ber` `beer`
 /be+r/g
 ```

## Question Mark `?`
- To indicate that a character is optional, we put a ? question mark after a character.

indicate that the following letter `u` is optional
```
color, colour

`color`, `colour`
/colou?r/g

a, an
`a`, `an`
/an?/g
```

## Curly Braces - 1
- To express a certain number of occurrences of a character.
- At the end we write curly braces `{n}` along with how many times we want it to occur.

 indicate that the following letter e can occur only 2 times in a row.
```
ber beer beeer beeeer

ber `beer` beeer beeeer
/be{2}r/g

Release 10/9/2021
Release 10/9/`2021`
/[0-9]{4}/g
```

## Curly Braces - 2
- To express at least a certain number of occurrences of a character.
- Immediately after the character we write at least how many times we want it to occur in a row followed by a comma `,` and wrapped inside curly braces `{n, }`

indicate that the following letter e can occur at least 3 times in a row.
```
ber beer beeer beeeer

ber beer `beeer` `beeeer`
/be{3,}r/g

Release 10/9/2021
Release `10`/9/`2021`
/[0-9]{2,}/g
```

## Curly Braces - 3
-  To express the occurrence of a character in a certain number range.
- We write curly braces `{x,y}` for the inclusive interval `x` for start, `y` for end.

indicate that the following letter e can only occur between 1 and 3 times in a row.
```
ber beer beeer beeeer

`ber` `beer` `beeer` beeeer
/be{1,3}r/g

Release 10/9/2021
Release `10`/`9`/`2021`
/[0-9]{1,4}/g
```

# Grouping
## Parentheses `( )`: Grouping
- We can group an expression and use these groups to reference or enforce some rules.
- To group an expression, we enclose () in parentheses.

group `haa` together
```
ha-ha,haa-haa
ha-ha,`haa`-`haa`
/(haa)/g

`ha`-`ha`,`ha`a-`ha`a
/(ha)/g
```

## Referencing a Group
- We want to reference the groups `(ha)` and `(haa)` in the pattern,
- we use `\1` for the first group and `\2` for the second group.
- In the pattern `(ha)-\1,(haa)-\2`, the `-` is outside the groups, and `\1` refers to `ha` while `\2` refers to `haa`. Type `\2` at the end of the expression to refer to the second group.
```
ha-ha,haa-haa

`ha-ha,haa-haa`
/(ha)-\1,(haa)-\2/g

`ha-ha,haa-ha`a
/(ha)-\1,(haa)-\1/g

`ha-ha`,haa-haa
/(ha)-\1/g

ha-ha,`haa-haa`
/(haa)-\1/g
```

## Parentheses `(?: )`: Non-capturing Grouping
- You can group an expression and ensure that it is not captured by references.
- For example, below are two groups.

 the first group reference we denote with `\1` actually indicates the second group, as the first is a non-capturing group.
 ```
ha-ha,haa-haa

`ha-ha,haa-haa`
(?:ha)-ha,(haa)-\1

`ha-ha,haa`-haa
(?:ha)-ha,(haa)-
 ```

# Alteration
## Pipe Character `|`
- It allows to specify that an expression can be in different expressions.
- Thus, all possible statements are written separated by the pipe sign `|`
- This differs from charset `[abc]`, charsets operate at the character level; Alternatives are at the expression level.

the following expression would select both cat and rat. Add another pipe sign | to the end of the expression and type dog so that all words are selected.

```
cat rat dog

`cat` `rat` `dog`
/(c|r)at|dog/g

`cat` `rat` dog
/(c|r)at/g

cat `rat dog`
(c|r)at dog
```

## Escape Character `\`
- There are special characters that we use when writing regex.
- `{ } [ ] / \ + * . $^ | ?` Before we can select these characters themselves, we need to use an escape character `\`

To select the dot `.` and asterisk `*` characters in the text, let's add an escape character `\` before it.
```
(*) Asterisk.

(`*`) Asterisk`.`
/(\*|\.)/g
```

# String
## Start of the String
-  Caret Sign `^`: Selecting by Line Start

To find only numbers at the beginning of a line, prefix this expression with the `^` sign
```
Basic Omellette Recipe

1. 3 eggs, beaten
2. 1 tsp sunflower oil
3. 1 tsp butter
   
`1`. 3 eggs, beaten
`2`. 1 tsp sunflower oil
`3`. 1 tsp butter
/^[0-9]/gm
```

## End of The String
- Dollar Sign `$`: Selecting by End of Line

Use the `$` sign after the html value to find the html texts only at the end of the line

```
https://domain.com/what-is-html.html
https://otherdomain.com/html-elements
https://website.com/html5-features.html

https://domain.com/what-is-html.`html`
https://otherdomain.com/html-elements
https://website.com/html5-features.`html`

/html$/gm
```

# Characters
## Word Character `\w`: Letter, Number and Underscore
- The expression `\w` is used to find letters, numbers and underscore characters
```
abcABC123 _.:!?

`a`b`c`A`B`C`1`2`3` `_`.:!?
/\w/g
```

## Except Word Character `\W`
- The expression `\W` is used to find characters other than letters, numbers, and underscores.
```
abcABC123 _.:!?

abcABC123` `_`.`:`!`?`
/\W/g
```

## Number Character `\d`
- `\d` is used to find only number characters.

```
abcABC123 _.:!?

abcABC`1`2`3` _.:!?
/\d/
```
## Except Number Character `\D`
- `\D` is used to find non-numeric characters.

```
abcABC123 _.:!?

`a`b`c`A`B`C`123` `_`.`:`!`?`
/\D/g
```

## Space Character `\s`
- `\s` is used to find only space characters.

```
abcABC123 _.:!?

abcABC123` `_.:!?
/\s/g
```

## Except Space Character \S
- `\S` is used to find non-space characters.

```
abcABC123 .:!?

`a`b`c`A`B`C`1`2`3` `.`:`!`?`
/\S/g
```

# Lookarounds
- If we want the phrase we're writing to come before or after another phrase, we need to "lookaround".

## Positive Lookahead: `(?=)`

we want to select the hour value in the text.
Therefore, to select only the numerical values that have PM after them, we need to write the positive look-ahead expression `(?=)` after our expression.
Include PM after the `=` sign inside the parentheses.
```
Date: 4 Aug 3PM

Date: 4 Aug `3`PM
/\d+(?=PM)/g
```

## Negative Lookahead: `(?!)`

we want to select numbers other than the hour value in the text.
Therefore, we need to write the negative look-ahead `(?!)` expression after our expression to select only the numerical values that do not have PM after them.
Include PM after the `!` sign inside the parentheses.
```
Date: 4 Aug 3PM

Date: `4` Aug 3PM
/\d+(?!PM)/g
```

## Positive Lookbehind: `(?<=)`

For example, we want to select the price value in the text.
Therefore, to select only the number values that are preceded by `$`, we need to write the positive lookbehind expression `(?<=)` before our expression. 
Add `\$` after the `=`  sign inside the parenthesis.
```
Product Code: 1064 Price: $5

Product Code: 1064 Price: $`5`
/(?<=\$)\d+/g
```

## Negative Lookbehind: `(?<!)`
we want to select numbers in the text other than the price value. 
Therefore, to select only numeric values that are not preceded by `$`, we need to write the negative lookbehind `(?<!)` before our expression. 
Add `\$` after the `! inside the parenthesis.

```
Product Code: 1064 Price: $5

Product Code: `1064` Price: $5
/(?<!\$)\d+/g
```

# Flags
- Flags change the output of the expression. 
- That's why flags are also called modifiers. 
- Flags determine whether the typed expression treats text as separate lines, is case sensitive, or finds all matches.
## Global Flag
- The global flag causes the expression to select all matches.
- If not used it will only select the first match. 
- `/g` is the symbol used as global flag

```
domain.com, test.com, site.com

`domain.com`, `test.com`, `site.com`
/\w+\.com/g
```

## Multiline Flag
- Regex sees all text as one line. 
- But we use the multiline flag to handle each line separately. 
- In this way, the expressions we write to identify patterns at the end of lines work separately for each line.
- `/m` is the symbol used as multiline flag

```
domain.com
test.com
site.com

`domain.com`
`test.com`
`site.com`
/\w+\.com$/gm
```


## Case-insensitive Flag
- `/i` is the symbol used as case-sensitivity flag

In order to remove the case-sensitivity of the expression we have written, we must activate the case-insensitive flag.

```
DOMAIN.COM
TEST.COM
SITE.COM

DOMAIN.COM
TEST.COM
SITE.COM
/\w+\.com$/gmi
```

# Matching
## Greedy Matching
- Regex does a greedy match by default. 
- This means that the matching will be as long as possible.

It refers to any match that ends in `r` and can be any character preceded by it. But it does not stop at the first match

```
ber beer beeer beeeer

`ber beer beeer beeeer`
/.*r/
```

## Lazy Matching
- Lazy matching, unlike greedy matching, stops at the first matching.
- For example, in the example below, add a `?` after `*` to find the first match that ends with the letter `r` and is preceded by any character. 
- It means that this match will stop at the first letter `r`.

```
ber beer beeer beeeer

`ber` beer beeer beeeer
/.*?r/
```