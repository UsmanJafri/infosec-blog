---
layout: post
title:  "Fundamentals of Regular Expressions"
date:   2020-09-01 19:50:00 +0500
categories: regex
---
Hello!

In today's blog I will be covering a very exciting topic, **Regular Expressions**!

Regular expressions, or more commonly known as *regexes* are patterns that are used to describe sequences of text.

**Here's a regular expression to describe an email address:**

```
\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,4}\b
```

**Demo:**

![](https://imgur.com/P1ucDL9.jpg)

**Description of the email address regular expression:**

The first part of the email address allows a sequence of any length greater than 1 of the alphabets A to Z in both upper and lower cases, numbers 0 to 9, and the following characters `.`, `_`, `%`, `+`, `-`. This sequence is then followed by a single `@` which then followed by sequence of any length greater than 1 of the alphabets A to Z in both upper and lower cases, numbers 0 to 9, and the charachers `.` or `-`, which is then followed by a single `.`, which is finally followed by a sequence of upper or lower case alphabets A to Z but of length greater than or equal to 2 and less than or equal to 4.

Regular expressions are commonly used for searching specific strings in large textual files or documents. Some popular applications of regular expressions include:

1. Code compilers and interpreters
2. Search engines
3. Code editors
4. Data analysis (such as filtering large log files)

Various regular expression interpreters exist and although they all follow the same basic principles, there can be minor differences between them (such as choice of escape characters) and for this reason regular expressions created for one interpreter may not always work with another. For all the examples in this tutorial, I will be using regular expressions that are compatible with the **PCRE (PHP)** flavor available at the excellent online regular expression tester available at **regex101.com**.

On to some regular expression fundamentals:

1. **Literal characters**

    The simplest form of a regular expression. As the name suggests, it would match the exact sequence in the regular expression everywhere it occours in a document.

    Here's an example:

    ![](https://imgur.com/cmd520l.jpg)

2. **Special characters**

    Special characters, also-known-as *Meta-characters*, are characters that are **NOT** interpreted literally and are reserved by the interpreter for special use cases.

    In order to use the reserved meta-characters ***literally***, you need to *escape* the characher with another special-character, the back-slash: `\`.

    All the following regular expressions fundamentals are dependent on special characters.

3. **Character sets using `[` and `]`**

    Anything inside the square brackets represents the set of characters allowed at one position in the regular expression. Note that only a **single** character from the set is matched.

    For example the regular expression `[abc]` will essentially behave like three individual literal regular expressions:

    - `a`
    - `b`
    - `c`

    Here's an example, notice that all the matches are of a single length only. We will explore how to use character sets for longer sequence matches later.

    ![](https://imgur.com/4rpYTrk.jpg)

4. **Negated character sets using `^`**

    Similar to character sets but this regular expression will match anything that is NOT in the character set. Here's the same test string from the previous example but with a negated character set:

    ![](https://imgur.com/DgIpe39.jpg)

5. **Character set sequences using `*`, `+`, and `?`**

    - `*` -> match between 0 and unlimited times

    - `+` -> match between 1 and unlimited times

    - `?` -> match between 0 and 1 time

    - No special character -> match exactly 1 time

    Here are a few examples to make things clearer:

    ![](https://imgur.com/pcAxhUB.jpg)

    Notice how the lone `!` and `400!` are both valid matches because we used the `*` operator with `[0-9]` which means a sequence of numbers of length 0 (that is, there are no numbers in the sequence) is a valid match too!

    `400` does not match because it does not have a trailing `!` which we did not couple with any special character and hence the regular expression requires exactly one instance of `!`.

    In this next example, we replace the `*` with a `+`. Notice that the lone `!` is no longer matched because we now require 1 or more instances of the numbers 0 to 9.

    ![](https://imgur.com/ZMibUUj.jpg)

    Next, we replace the `+` with a `?`. Now notice that the lone `!` is being matched again but only `0!` is being matched in the line containing `400!`.

    ![](https://imgur.com/fbocQ0u.jpg)

    *Why is this?* Recall that `?` matches 0 or 1 time only. So for the lone `!` there are 0 instances of numbers hence it is a valid match. For the line containing `400!` only 1 instance of a number is required, along with a single instace of the `!` character to count as a match.

6. **Word Boundaries**

    Beginning and ending of words can be signified using the `\b` special character.

    Here are a few examples:

    ![](https://imgur.com/p1FH5Gi.jpg)

    Notice that for a match to occour a word should start with `hello` regardless of how it ends. This is why `hellothere` is being considered a match.

    Now if we wanted to match `hello` **ONLY** we could add a word boundary at the end too, like so:

    ![](https://imgur.com/b4DLo9j.jpg)

    What if wanted to match `hello` at the end of a word only? Here:

    ![](https://imgur.com/0fYkykf.jpg)

7. **String Anchors**

    Unlike word boundaries, string achors are used to signify the starting and ending of strings.

    - `^` -> start of string

    - `$` -> end of string

    Here are a few examples using the same test string as previous:

    String ending with `hello`:

    ![](https://imgur.com/8AjleuQ.jpg)

    String starting with `hello`:

    ![](https://imgur.com/jTbTDIo.jpg)

    String that occours exactly as, starts with and ends with `I hellothere`. Notice that `Hey I hellothere` is **NOT** being matched:

    ![](https://imgur.com/e9pfmq8.jpg)

8. **Atomic Groups**

    Let's say I wanted to match the words `bike` and `bicycle` **literally** but using a single regular expression, how would I do that? Atomic groups!

    Here's an example where I group `ke` and `cycle` into a single atomic group and hence can match both `bike` and `bicycle` using a single regular expression!

    ![](https://imgur.com/Gc3LFbY.jpg)

9. **Line-breaks, carriage returns and more!**

    Note that when say ***text***, we mean anything that can be represented in **ASCII**. Therefore, special characters that are not-printable but can be encoded in ASCII such as `\t` for tab, `\n` for line break and `\r` for carriage returns can be used in regular expressions too.

    Here's an example to find a line that starts with `I hellothere` and ends with two new lines:

    ![](https://imgur.com/evI3jI1.jpg)

Regular expressions can be quite powerful (and complex too!) and I'll be honest, we have only explored the tip of the ice-berg of regular expressions in this post, but armed with these fundamentals we can definitely begin to explore other harder regular expressions!