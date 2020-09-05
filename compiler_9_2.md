---

title: Notes 9/2
permalink: /compiler/9_2

---

# 9/2 Compiler Notes

# Regular Expressions Cont'd

## Exercises
All strings of lowercase letters that either begin or end in a (or both)
> a[a-z]* \| [a-z]*a


All strings of digits that contain no leading zeros.
> 0 \| [1-9]*[0-9]*

All strings of digits that represent even numbers and no leading zeros

(integers ending with 0,2,4,6,8)

> [1-9][0-9]* [02468] \| 02468

All strings of digits such that all the 2's occur before all the 9's
> [0-8]*[013-9]*

All strings of a's and b's that contain no three consecutive b's.
> (b\|bb)?(a\|ab\|abb)*
or
> (a\|ba\|bba)*(b\|bb)?

All strings of a's and b's that contain an odd number of a's.
> b*(ab*ab*)*

All strings of a's and b's that contain an even number of a's.
> b*(ab*ab*)*

All strings of a's and b's that contain an even number of a's and an even number of b's.
> (aa\|bb)*( (ab\|ba)(aa\|bb)*(ba\|ab) )*(aa\|bb)* 

Flex: a scanner generator

format of Flex input file:
    contains three components separated by %%

1st component

    specify Flex options
    
    code block:
    %{
        .. //c++ code that will be copied to lex.yy.cc file declaration of global variables, function prototypes, include header files.
    %}
    
    definitions of regular expressions
    
    %%
    
    2nd component
    rules
    RE: {actions}
    
    %%
    
    3rd component
    Helper code in c++ or c, or java, depending on what type of scanner to be generated.
