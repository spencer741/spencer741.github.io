
---
layout: page
title: Notes 8/31
permalink: /compiler/
---


8/31
Project 1:
PrintStm::interp(...){
    call the interp of the explist object
}

PairExpList::interp(...){
    evaluate head and output the return value
    call interp function through tail
}

LastExpList::interp(...){
    evaluate head and ouput the return value
}

all of the values of the expression list should be outputted on the same line ...


## Regular expressions
    a regular expression represents a string pattern

    language of a regular expression
    l(r) = the set of all strings that satisfy the pattern specified by r

#####Recursive definition of regular expressions:
Basic regular expression:
1) each symbol in the alphabet is a regular expression
2) empty string is also a regular expression. Epsilon is used to represent an empty string. Epsilon cannot occur in the alphabet.

>L(a) = { a }

>L($\epsilon$) = { $\epsilon$ }




---
There are three operators we can use to constuct more complex regular expressions. For the following, assume r1 and r2 are regular expressions.

---

###1. Choice operator \{ | }

`r1 | r2` is *also* a regular expression (because `r1` and `r2` are independently regular expressions).

What is the pattern represented in this regular expression?

>L(r1|r2) = L(r1) U L(r2)

If the string matches the pattern of `r1` **OR** the pattern of `r2`,
then it can be recognized by the entire regular expression.

>L(a|b) = { a,b } 

Also, the order does not matter. `L(b|a)` is the same as `L(a|b)`.

Other examples:

>L(a|a) = { a }  &nbsp;&nbsp;&nbsp;&nbsp;... this can be reduced to just L(a)

>L(a|$\epsilon$) = { a,$\epsilon$ } &nbsp;&nbsp;... either a or empty string.

### 2) Concatenation Operator \{ x }
`r1r2` is *also* a regular expression (because `r1` and `r2` are independently regular expressions).
>L(r1,r2) = L(r1) x L(r2)

`{1,2} x {a,b,c}` can be defined as all of the possible combinations of the set in the left and the set in the right. `{ 1a, 1b, 1c, 2a, 2b, 2c }`.

Does the order matter with the Concatenation Operator?
**Yes! The order matters!**

>L(ab) = L(a) x L(b) ... { a } x { b } ... {ab}

>L(a$\epsilon$) = { a } ... because (a) followed by an empty string ($\epsilon$) is still (a).

A more complex example...
>L(a(b|c)) = L(a) x L(b|c) = { a } x { b,c } = { ab, ac }

Can a string be used to recognize the concatentation of two regular expressions?
Yes. If you are able to parse the string, you can find a way to partiton the string into two parts. If both partitions match, then you have acheived it. If not, you would iterate and test different partitions of the string.
These are very basic examples:
>a|b|c|d

>ab can be concatenated with c to make abc. ab can be concatenated with cd to make abcd ...  and so forth. 

### 3) closure or repetition operator \{ * }
`r1*` is a regular expression
This operator is used to represent the repetition of `r1` as many times as needed.

So does this mean 0 or more times or does it mean 1 or more times?
The \* operator means 0 or more times.

>L(r*) = L($\epsilon$) ... this is just an empty string

>L(r*) = L($\epsilon$) U L(r) ... this is one r

>L(r*) = L($\epsilon$) U L(r) U L(rr) ... this is two or more r's

>L(a*) = { $\epsilon$,a,aa,aaa,... }

If you apply the closure operator to $\epsilon$, it still evaluates to empty string...no matter how many times you repeat it.

>L($\epsilon$*) = { $\epsilon$ } 

More complex example:
>L((a|b)*) = { $\epsilon$ } repeat 0 times
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= { a,b } repeat 1 times
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= { aa,bb,ab,ba } repeat 2 times
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= { bbb,bba,bab,baa, } repeat 3 times

In the above example, as long as the string contains a or b, it can be recognized.

Now let's look at another example:
>a|bc* 

Well, this is not clear ... enter precedence level!

### Precedence level

Lowest ------------------------------ Highest
**choice**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**concatenation**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**repetition**

And of course you can always use parenthesis to change precedence, like you see in mathematical notation.

Thus, the following expression
>a|bc*

can become

>a|(b(c*))

This particular expression can recongize `a` or a string that starts with `b` followed by any number of `c`'s.

## Regular Expression Extensions
These extensions doesn't increase the poewr of regular expressions

These extensions help us write regular expressions for a given pattern (or should I say it's more convinient/succinct... extensions just make the job easier for us).

**+**
means to repeat at least once.

>r+ = rr*  (here = means equivalent to)

>L(a+) =  { a,aa,aaa }
>L(a*) = { $\epsilon$ } U L(a+)

**?**
means that the preceding pattern can occur zero **or** once

>r? = r | $\epsilon$

**.**
matches any individual symbol in the alphabet (except newline character). It also matches an empty string.

>(a|b|c)* is equivalent to .*



**[ ]** 
is an alternative to the choice operator.

Assume `a1 a2 a3 , ... ,` are all symbols in the alphabet.

>[a1a2a3a4...an] = a1 | s2 | a3 | ... | an

>L([abc12]) = { a,b,c,1,2 }

**-**
represents range if used within `[ ]`
>L([0-9]) = { 0,1,2,3,4,5,6,7,8,9 }

>[a-z] [A-Z] [a-Z] [a-zA-Z]

**^**
If put as the first character within `[ ]`, it means **complement**.
[^a1a2a3...an]: matches any individual symbol that is not listed in here explicitly.

If the alphabet is 
> $\sum$ = {a,b,c,d,e}

>L([^abc]) = {d,e} any symbol other than a,b or c

>[^jane].* any string of letters that does not start with jane.

**Remember...** epsilon $\epsilon$ is never part of any alphabet.

Note that you can also assign names to regular expressions to name them via assignment or reference them in other regular expressions.

**Special Characters...**
`\n` new line character
`\t` tab character
`" "`: any symbol in within a double quoted string has no special meaning.
&nbsp;&nbsp;&nbsp;&nbsp;`"a*"` --------> `a*`
&nbsp;&nbsp;&nbsp;&nbsp;`"."`  --------> `.` only recognizes the character . (dot)
&nbsp;&nbsp;&nbsp;&nbsp;`"\t"` --------> Special tab character
&nbsp;&nbsp;&nbsp;&nbsp;`"\\t"` -------> `\t` (this is a string of two characters, not the special tab character)
&nbsp;&nbsp;&nbsp;&nbsp;`"\\\\"` ------> `\\`

## Exercises
Write a regular expression to satisfy the following:

Q: All strings of lowercase letters that begin and end in a.
A: `a[a-z]*a | a`

{% include lib/mathjax.html %}
