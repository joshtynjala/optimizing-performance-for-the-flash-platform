# Regular expressions

> ![](../img/tip_help.png) Use String class methods such as `indexOf()`,
> `substr()`, or `substring()` instead of regular expressions for basic string
> finding and extraction.

Certain operations that can be performed using a regular expression can also be
accomplished using methods of the String class. For example, to find whether a
string contains another string, you can either use the `String.indexOf()` method
or use a regular expression. However, when a String class method is available,
it runs faster than the equivalent regular expression and does not require the
creation of another object.

> ![](../img/tip_help.png) Use a non-capturing group (" `(?:xxxx)` ") instead of
> a group (" `(xxxx)` ") in a regular expression to group elements without
> isolating the contents of the group in the result.

Frequently in regular expressions of moderate complexity, you group parts of the
expression. For example, in the following regular expression pattern the
parentheses create a group around the text "ab." Consequently, the "+"
quantifier applies to the group rather than to a single character:

    /(ab)+/

By default, the contents of each group are "captured." You can get the contents
of each group in your pattern as part of the result of executing the regular
expression. Capturing these group results takes longer and requires more memory
because objects are created to contain the group results. As an alternative, you
can use the non-capturing group syntax by including a question mark and colon
after the opening parenthesis. This syntax specifies that the characters behave
as a group but are not captured for the result:

    /(?:ab)+/

Using the non-capturing group syntax is faster and uses less memory than using
the standard group syntax.

> ![](../img/tip_help.png) Consider using an alternative regular expression
> pattern if a regular expression performs poorly.

Sometimes more than one regular expression pattern can be used to test for or
identify the same text pattern. For various reasons, certain patterns execute
faster than other alternatives. If you determine that a regular expression is
causing your code to run slower than necessary, consider alternative regular
expression patterns that achieve the same result. Test these alternative
patterns to determine which is the fastest.
