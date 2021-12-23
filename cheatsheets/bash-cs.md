# Bash Scripting					
                    
                
## Parameter options

	$0						Name of script
    $#						Number of parameters
	$*						All parameters
	$@						Like $* with quotes separating each parameter
	$1, $2....						Parameters

	shift						Move parameters down 1 (1=$2, 2=$3 etc)
	shift <x>						Move parameters along by x

## Core commands

	sort [options] <filename>				Sort a file
		-o					Specify output file (can be same as 1 of input files)
		-u					Unique output only 

		-f					Lowercase and uppercase treated the same
		-i					Ignore non-printable chars
		-r					Reverses order
		-n					Numeric order
		-k field_start [type] [,field_end [type] ]	Sort by key field
	
	cut						Cut columns of outputted text
		-b<x>-<y>				Byte positions x to y
	-c<x>-<y>				Character positions x to y
		-d<delim>				Field delimited (default is tab)
		-f<list of field numbers>			Field list, separated by delimeter as specified


## Loops

    packlist=`cat rawsourcelist.asc`			
    for pack in $packlist
    do
    printf "Processing $pack\n"
    done


## Reading from files

Read lines from /export/home/michelep/longpacklist.asc and processes the lines

    while read packname; do
    read instpath;instpath=`echo $instpath|sed "s/installed pathnames//"|sed "s/\n//"`
    read blocks;blocks=`echo $blocks|sed "s/used (approx)//"|sed "s/\n//"`
    printf "${packname}\t${blocks}\t${instpath}\n"
    done < /export/home/michelep/longpacklist.asc

This example reads input from a file filtering out comment lines (begin with #)
	
	grep “^[^#]” /etc/configfile | {
		while read conf_line; do
			printf “$conf_line\n”
	done	
	}


## Read from the screen

	read name1 name2 name3			Read 3 variables from stdin
	read -p "username: " user4		Read variable with a prompt
        -a					        Read values into an array


If no variable is specified it goes into $REPLY variable


## Case Statements

	case expression in
		pattern1 )
			statements ;;
		pattern2 )
			statements ;;
		.....
	esac


## Processing Text

Sed on a variable to remove spurious text and EOL codes
	instpath=`echo $instpath|sed "s/installed pathnames//"|sed "s/\n//"`	

If based on lines that don’t begin with a comment:
	if [[ $line != \#* ]]; then

Find section of code
		sed '1,/firstmatch/d;/secondmatch/,$d'


## Arithmetic

	width=$(( $width + 2 ))				Addition
length=${#newstr}				Get length of newstr
echo $(( $num1 + $num2 ))
	echo $(( $a * 3 ))
echo $(expr $num1 + $num2 )

for complicated arithmetic pipe it to bc
		echo "21^3"|bc


## Simple Menus

    select name in julia isabella fiona 
    do
        echo $name
    done



## System Variables

	$LOGNAME					Login name
	$COLUMNS					Screen width
	$LINES						Number of lines on screen
	$PS1						Prompt

## Arrays

	set -A filenames $(ls $@)				$filename is an array of files as passed by params
	fname=${filenames[$count]}			    Read an element from the array
	count=${filenames[*]}				    Set count to number of elements in the array

    os=('Ubuntu' 'RedHat' 'CentOS')			Spaces not commas in definition
    echo "${os[@]}"					        Print all elements array
    echo "${os[0]}"					        Print 1st element 
    echo "${!os[@]}"					    Print element indices
    echo ${#os[@]}"					        Number of elements in array
    os[3]='mac'					            Set another element
    unset os[2]					            Unset an element


## Functions

	function func-name {
		......
		echo $1 $2 $3				Arguments
	}

    function func-name () {
        ......
    }

    eg
        function sayhello () {
        echo “Hello $1”
        }
        sayhello Bob

        $ functions					Show defined functions
        $ typeset -f					Show defined functions
        local <var allocation>				Local variable definition


## Print/printf

	printf <format> <arguments>			Print text

	Formats:
		\b					Backspace cursor
		\n					Newline
		\t					Tab
		\f					Formfeed
		%d					Print argumnent as decimal
		%x					Print argumnent as hex
		%s					Print argumnent as string

		%-6d					- = Left justify in field, 6=min field width



## Operators

	${varname:-word}				If varname exists and isn’t null return it, else return word
	${varname:?message}				If varname exists return it, else return “varname:” message
	${varname:+word}				If varname exists and isn’t null return word, else return null


## Conditionals

	if [ EXPRESSION ] ; then 
        DO THIS 
        ... 
    elif [ EXPRESSION ] ; then        # else if is optional, and there can be more than 1 of them 
        DO THIS 
        ... 
    elif 
        ... 
    else                        # else is optional, only one per if statement 
        DO THIS 
        ... 
    fi       

	<string1> = <string2>				True if strings are equal
	<string1> != <string2>				True if strings are unequal
	<string1> </> <string2>				True if string1 is < or > string2 alphabetically.
	<exp1> -lt/-ge/-le/-ge/-eq <exp2>			Comparison for values/expressions
	< / > / <= / >=					As above but need to be enclosed in double brackets

	-z <string>					String length = 0
	<string>	 					String length non-zero
	
	-e <filename>					True if file exists
	-d <dirname>					True if file exists and is a directory
	-f <dirname>					True if file exists and is a regular file
	-w <dirname>					True if file exists and is writeable
	-x <dirname>					True if file exists and is executable
    -h <file>						True if file exists and is a symbolic link
	-s <filename>					File exists and is >0 size

	if [ $a - lt $b ]					3 Equivalent conditions
	if (( $a < $b ))
	if [[ $a < $b ]]

## Debugging

    $ bash -x <script>			Run in debug more

    set -x inside script – debug from that point. Set +x will deactivate debugging.

    put “-x” on the #!/bin/bash line of the script
