# SED basics

Probably pretty much all you are likely to need to know with sed
If it's not obvious what soemthing does just type it at the command prompt

## Simple Replace

Replace 1st pattern with 2nd pattern

    echo "abcdefghi"| sed 's/def/ - /'      # Replace 1st string with 2nd string - 1st instance only 
    echo "abcabcabc"| sed 's/b/B/g'         # Global replace - all occurrences

## Pattern substitution

\\( and \\) to bound a pattern, then \1 \2 etc to replace with the found pattern

    echo "the cat sat on the mat"| sed 's/\(c[a-z]*\)/-\1\1-/g'             # Replace "cat" with -catcat-
    echo "the cat sat on the mat"| sed 's/\(c[a-z]*\)/\U\1/g'               # Make the pattern uppercase

## Stripping specific section of a file

Strip section out of file between 2 boundaries

    printf "a\nb\nc\nd\ne\nf\ng"|sed -n '/c/,/e/p'

The following will strip all lines before a pattern, but wil strip the pattern match line

    cat sed-cs.md |sed  '1,/## Pattern substitution/d'

And similarly just show lines upto the pattern:

    cat sed-cs.md |sed  '/## Pattern substitution/,$d'

## Basics

* Single or double quotes depenting on whether you need variable interpolation or not

* The usual separator is "/" but you can use pretty much anything, so the following are all the same work and make it easier to work with slashes:

    echo "abcabcabc"| sed 's/b/B/g'
    echo "abcabcabc"| sed 'sZbZBZg'
    echo "abcabcabc"| sed 's_b_B_g'

* Newlines can be inserted using single space and physically typing \ before newline, eg:

    echo "abcabcabc"| sed 's/ca/_\
    /'

* Chaining multiple commands can be done with semicolon

    echo "abcdefghi"| sed 's/c/_/;s/f/-/'
