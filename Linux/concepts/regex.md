# <center> Regex Tutorial - How to write Regular Expressions </center>

A regular expression (regex) is a sequence of characters that defines a search pattern. It is mainly used for pattern matching in strings, such as finding, replacing, or validating text

![regex](https://media.geeksforgeeks.org/wp-content/uploads/20251220171423685683/example_of_regular_expression.webp)


Anchors enforce position in the string:

- `^` = Start of the string.
- `$` = End of the string.
---

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