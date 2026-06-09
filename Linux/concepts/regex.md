# <center> Regex Tutorial - How to write Regular Expressions </center>

A regular expression (regex) is a sequence of characters that defines a search pattern. It is mainly used for pattern matching in strings, such as finding, replacing, or validating text

![regex](https://media.geeksforgeeks.org/wp-content/uploads/20251220171423685683/example_of_regular_expression.webp)

* **Character Classes**: Distinguish kinds of characters, such as letters and digits.
* **Quantifiers**: Control the number of characters to match. 
* **Lookarounds**: Match characters only if they are preceded or followed by other characters.
* **Capturing Groups**: Match characters that are contained within the group. 
* **Named Capturing Groups**: Match characters that are contained within the group and name them. 

Anchors enforce position in the string:

- `^` = Start of the string.
- `$` = End of the string.
---
![regex](https://github.com/ziishaned/learn-regex/raw/master/img/regexp-en.png)

## Common Elements Used in Regular Expressions

1. **Repeaters ( `*, +, {}`)**: Repeaters specify how many times the preceding character or group should appear. 

2. **Asterisk Symbol `*`**: Matches the preceding character `0` or more times. 
    <p>EXAMPLE: The regular expression ab*c will give ac, abc, abbc, abbbc….and so on</p>

3. **Plus Symbol `+`**: Matches the preceding character `1` or more times. 
    <p>EXAMPLE: The regular expression ab+c will give abc, abbc, abbbc, … and so on.</p>

4. **Curly Braces `{}`**: Defines an exact or range of repetitions. 

5. **Wildcard `.`**: Matches any single character except a newline. 

6. **Optional Character `?`**: Matches `0 or 1` occurrence of the preceding character. 
    <p>EXAMPLE: docx? matches doc and docx</p>

7. **Caret `^` Symbol**: Ensures the match starts at the beginning of the string. 

8. **Square `[]`**: `[abc] matches a, b or c`

9. **Negated Character Class `[^ ]`**: Matches characters not listed in the brackets. 
    <p>[^abc] -> matches any character except a,b,c </p>

<table><thead><tr><th>Quantifier</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><code>*</code></td><td>0 or more occurrences (greedy)</td><td><code>a*</code> → matches "", "a", "aa"</td></tr><tr><td><code>+</code></td><td>1 or more occurrences (greedy)</td><td><code>a+</code> → matches "a", "aa" (not "")</td></tr><tr><td><code>?</code></td><td>0 or 1 occurrence (greedy)</td><td><code>a?</code> → matches "" or "a"</td></tr><tr><td><code>{n}</code></td><td>Exactly <code>n</code> occurrences</td><td><code>a{3}</code> → matches "aaa"</td></tr><tr><td><code>{n,}</code></td><td><code>n</code> or more occurrences</td><td><code>a{2,}</code> → "aa", "aaa"</td></tr><tr><td><code>{n,m}</code></td><td>Between <code>n</code> and <code>m</code> occurrences (inclusive)</td><td><code>a{1,3}</code> → "a", "aa", "aaa"</td></tr></tbody></table>

---
# 2. Meta Characters

Meta characters are the building blocks of regular expressions. Meta characters do not stand for themselves but instead are interpreted in some special way. Some meta characters have a special meaning and are written inside square brackets.

|Meta character|Description|
|:----:|----|
|.|Period matches any single character except a line break.|
|[ ]|Character class. Matches any character contained between the square brackets.|
|[^ ]|Negated character class. Matches any character that is not contained between the square brackets|
|*|Matches 0 or more repetitions of the preceding symbol.|
|+|Matches 1 or more repetitions of the preceding symbol.|
|?|Makes the preceding symbol optional.|
|{n,m}|Braces. Matches at least "n" but not more than "m" repetitions of the preceding symbol.|
|(xyz)|Character group. Matches the characters xyz in that exact order.|
|&#124;|Alternation. Matches either the characters before or the characters after the symbol.|
|&#92;|Escapes the next character. This allows you to match reserved characters <code>[ ] ( ) { } . * + ? ^ $ \ &#124;</code>|
|^|Matches the beginning of the input.|
|$|Matches the end of the input.|

### 2.1 The Full Stop

The full stop `.` is the simplest example of a meta character. The meta character `.` matches any single character. It will not match return or newline characters.

<pre>
".ar" => The <a href="#learn-regex"><strong>car</strong></a> <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

## 2.2 Character Set
Character sets are also called character classes. Square brackets are used to specify character sets.

Sometimes you may want to match a group of characters which are sequential in nature, such as any uppercase English letter. But writing all 26 letters would be quite tedious.

> Regex solves this issue with ranges. The `-` acts as a range operator

<table>
<thead>
<tr>
<td>Range</td><td>Matches</td></tr>
</thead>
<tbody>
<tr>
<td>[A-Z]</td><td>uppercase letters</td></tr>
<tr>
<td>[a-z]</td><td>lowercase letters</td></tr>
<tr>
<td>[0-9]</td><td>Any digit</td></tr>
</tbody>
</table>

<pre>
"[Tt]he" => <a href="#learn-regex"><strong>The</strong></a> car parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

A period inside a character set, however, means a literal period. The regular expression `ar[.]` means: a lowercase character `a`, followed by the letter `r`,
followed by a period `.` character.

<pre>
"ar[.]" => A garage is a good place to park a c<a href="#learn-regex"><strong>ar.</strong></a>
</pre>

- You are not limited to specifying only one range inside a character set. You can use multiple ranges and also combine them with any other additional character(s).
**Example**: `[3-6u-w;]` will match any of '3456uvw' or semicolon ';'.

### 2.2.1 Negated Character Set

If you prefix the set with a `^`, the inverse operation will be performed. For example, [^A-Z0-9] will match anything except uppercase letters and digits.

The regular expression `[^c]ar` means: any character except `c`, followed by the character `a`, followed by the letter `r`.

<pre>
"[^c]ar" => The car <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

## 2.3 Repetitions

The meta characters `+`, `*` or `?` are used to specify how many times a subpattern can occur. These meta characters act differently in different situations.

#### **The Star** `*`
- The `*` symbol matches zero or more repetitions of the preceding matcher.
- The regular expression `a*` means: zero or more repetitions of the preceding lowercase character a
- if it appears after a character set or class then it finds the repetitions of the whole character set.

<pre>
"[a-z]*" => T<a href="#learn-regex"><strong>he</strong></a> <a href="#learn-regex"><strong>car</strong></a> <a href="#learn-regex"><strong>parked</strong></a> <a href="#learn-regex"><strong>in</strong></a> <a href="#learn-regex"><strong>the</strong></a> <a href="#learn-regex"><strong>garage</strong></a> #21.
</pre>
`[a-z]*` means: any number of lowercase letters in a row.

#### **The Plus** `+`
- The `+` symbol matches one or more repetitions of the preceding character.
-  For example, the regular expression `c.+t` means: a lowercase c, followed by at least one character, followed by a lowercase *t*. It needs to be clarified that *t* is the last *t* in the sentence.

<pre>
"c.+t" => The fat <a href="#learn-regex"><strong>cat sat on the mat</strong></a>.
</pre>

#### **The Question Mark** `?`
- In regular expressions, the meta character `?` makes the preceding character optional. 
- This symbol matches zero or one instance of the preceding character. 
- For example, the regular expression `[T]?he` means: Optional uppercase T, followed by a lowercase h, followed by a lowercase e.
<pre>
"[T]he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in the garage.
</pre>

<pre>
"[T]?he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in t<a href="#learn-regex"><strong>he</strong></a> garage.
</pre>

## 2.4 Curly Braces `{}`
In regular expressions, braces (also called **quantifiers**) are used to specify the number of times that a character or a group of characters can be repeated.

For example, the regular expression `[0-9]{2,3}` means: *Match at least 2 digits, but not more than 3, ranging from 0 to 9*.

<pre>
"[0-9]{2,3}" => The number was 9.<a href="#learn-regex"><strong>999</strong></a>7 but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

We can leave out the second number. For example, the regular expression `[0-9]{2,}` means: Match 2 or more digits. If we also remove the comma, the regular expression `[0-9]{3}` means: Match exactly 3 digits.

<pre>
"[0-9]{2,}" => The number was 9.<a href="#learn-regex"><strong>9997</strong></a> but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

<pre>
"[0-9]{3}" => The number was 9.<a href="#learn-regex"><strong>999</strong></a>7 but we rounded it off to 10.0.
</pre>

<table>
<thead>
<tr>
<td>Quantifier</td><td>Matches</td></tr>
</thead>
<tbody>
<tr>
<td>*</td><td>0 or more</td></tr>
<tr>
<td>?</td><td>0 or 1</td></tr>
<tr>
<td>+</td><td>1 or more</td></tr>
<tr>
<td>{n}</td><td>exactly n times</td></tr>
<tr>
<td>{n, }</td><td>n or more times</td></tr>
<tr>
<td>{n, m}</td><td>n to m times inclusive</td></tr>
</tbody>
</table>

## 2.5 Capturing Groups
Capturing groups in regular expressions are a way to extract specific parts of a matched string. A capturing group is a group of subpatterns that is written inside parentheses `(...)`
- If we put a quantifier after a character then it will repeat the preceding character. But if we put a quantifier after a capturing group then it repeats the whole capturing group.
- For example, the regular expression `(ab)*` matches zero or more repetitions of the character "ab".
- We can also use the alternation `|` meta character inside a capturing group. For example, the regular expression `(c|g|p)ar` means: *a lowercase c, g or p, followed by a, followed by r.*

<pre>
"(c|g|p)ar" => The <a href="#learn-regex"><strong>car</strong></a> is <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

#### Non-Capturing Groups
A non-capturing group is a capturing group that matches the characters but does not capture the group.

A non-capturing group is denoted by a `?` followed by a `:` within parentheses `(...)`. For example, the regular expression `(?:c|g|p)ar`

<pre>
"(?:c|g|p)ar" => The <a href="#learn-regex"><strong>car</strong></a> is <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

## 2.6 Alternation

In a regular expression, the vertical bar `|` is used to define **alternation**.
> Alternation is like an `OR` statement between multiple expressions.

But the big difference between character sets and alternation is that character sets work at the character level but alternation works at the expression level.

<pre>
"(T|t)he|car" => <a href="#learn-regex"><strong>The</strong></a> <a href="#learn-regex"><strong>car</strong></a> is parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

## 2.7 Escaping Special Characters

A backslash `\` is used in regular expressions to escape the next character. 
- This allows us to include reserved characters such as `{ } [ ] / \ + * . $ ^ | ?` as matching characters. 
- To use one of these special character as a matching character, **prepend** it with `\`.

<pre>
"(f|c|m)at\.?" => The <a href="#learn-regex"><strong>fat</strong></a> <a href="#learn-regex"><strong>cat</strong></a> sat on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

## 2.8 Anchors 

In regular expressions, **anchors** are special tokens that don’t match characters — instead, they match positions in the text. They are used to ensure that a match occurs at a specific location in the string.

#### Caret `^`
The caret symbol `^` is used to check if a matching character is the first character of the input string.

<pre>
"(T|t)he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

<pre>
"^(T|t)he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in the garage.
</pre>

#### Dollar Sign `$`
The dollar sign `$` is used to check if a matching character is the last character in the string.

<pre>
"(at\.)" => The fat c<a href="#learn-regex"><strong>at.</strong></a> s<a href="#learn-regex"><strong>at.</strong></a> on the m<a href="#learn-regex"><strong>at.</strong></a>
</pre>

<pre>
"(at\.)$" => The fat cat. sat. on the m<a href="#learn-regex"><strong>at.</strong></a>
</pre>

---
# 3. Shorthand Character Set

To make the expression simpler, classes have been assigned to well-defined character groups such as digits. The following table shows these classes and their equivalent expression with character sets:

<table>
<thead>
<tr>
<td>Class</td><td>Matches</td><td>Equivalent expression</td></tr>
</thead>
<tbody>
<tr>
<td>.</td><td>anything except newline</td><td>[^\n\r]</td></tr>
<tr>
<td>\w</td><td>word character</td><td>[a-zA-Z0-9_]</td></tr>
<tr>
<td>\W</td><td>non-word character</td><td>[^\w]</td></tr>
<tr>
<td>\d</td><td>digits</td><td>[0-9]</td></tr>
<tr>
<td>\D</td><td>non-digits</td><td>[^\d]</td></tr>
<tr>
<td>\s</td><td>space, tab, newlines</td><td>[ \t\r\n\f]</td></tr>
<tr>
<td>\S</td><td>non whitespace characters</td><td>[^\s]</td></tr>
</tbody>
</table>

---
#  4. Lookarounds
Lookarounds in regular expressions are **zero-width assertions** — they check whether a certain pattern exists **before or after** the current position **without consuming characters** in the match.

### Types of Lookarounds

<table>
<thead>
<tr>
<th>Type</th>
<th>Syntax</th>
<th>Meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Positive Lookahead</strong></td>
<td><code>X(?=Y)</code></td>
<td>Match <code>X</code> <strong>only if</strong> it is <strong>followed by</strong> <code>Y</code>.</td>
</tr>
<tr>
<td><strong>Negative Lookahead</strong></td>
<td><code>X(?!Y)</code></td>
<td>Match <code>X</code> <strong>only if</strong> it is <strong>NOT followed by</strong> <code>Y</code>.</td>
</tr>
<tr>
<td><strong>Positive Lookbehind</strong></td>
<td><code>(?&lt;=Y)X</code></td>
<td>Match <code>X</code> <strong>only if</strong> it is <strong>preceded by</strong> <code>Y</code>.</td>
</tr>
<tr>
<td><strong>Negative Lookbehind</strong></td>
<td><code>(?&lt;!Y)X</code></td>
<td>Match <code>X</code> <strong>only if</strong> it is <strong>NOT preceded by</strong> <code>Y</code>.</td>
</tr>
</tbody>
</table>

<pre>
"(T|t)he(?=\sfat)" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

<pre>
"(T|t)he(?!\sfat)" => The fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

<pre>
"(?<=(T|t)he\s)(fat|mat)" => The <a href="#learn-regex"><strong>fat</strong></a> cat sat on the <a href="#learn-regex"><strong>mat</strong></a>.
</pre>

<pre>
"(?&lt;!(T|t)he\s)(cat)" => The cat sat on <a href="#learn-regex"><strong>cat</strong></a>.
</pre>

---
# 5. Flags
Regular expression flags (also called **modifiers**) change how a regex pattern is interpreted and matched.

They are appended after the closing slash of the pattern, `/pattern/flags` 

|Flag|Description|
|:----:|----|
|i|Case insensitive: Match will be case-insensitive.|
|g|Global Search: Match all instances, not just the first.|
|m|Multiline: Anchor meta characters work on each line.|

### 5.1 Case Insensitive
The `i` modifier is used to perform case-insensitive matching.
<pre>
"The" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

<pre>
"/The/gi" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

### 5.2 Global Search
The `g` modifier is used to perform a global match (finds all matches rather than stopping after the first match).

<pre>
"/.(at)/" => The <a href="#learn-regex"><strong>fat</strong></a> cat sat on the mat.
</pre>

<pre>
"/.(at)/g" => The <a href="#learn-regex"><strong>fat</strong></a> <a href="#learn-regex"><strong>cat</strong></a> <a href="#learn-regex"><strong>sat</strong></a> on the <a href="#learn-regex"><strong>mat</strong></a>.
</pre>

### 5.3 Multiine
The `m` modifier is used to perform a multi-line match.

<pre>
"/.at(.)?$/" => The fat
                cat sat
                on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

<pre>
"/.at(.)?$/gm" => The <a href="#learn-regex"><strong>fat</strong></a>
                  cat <a href="#learn-regex"><strong>sat</strong></a>
                  on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

---
# 6. Greddy vs Lazy Matching

By default, a regex will perform a **greedy** match, which means the match will be as long as possible. 
> We can use `?` to match in a lazy way, which means the match should be as short as possible.

<pre>
"/(.*at)/" => <a href="#learn-regex"><strong>The fat cat sat on the mat</strong></a>. </pre>

<pre>
"/(.*?at)/" => <a href="#learn-regex"><strong>The fat</strong></a> cat sat on the mat. </pre>

---