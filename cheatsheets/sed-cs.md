# SED basics

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


