Recall: looping over a list 

~~~ bash 
#!/bin/bash 
for name in .cpp, do           # the glob *.cpp is replaced inline 
	mv ${name} ${name%cpp}cc   # with every .cpp file in cwd
done 
~~~

The `for ...in...` syntax sets the var to each word in the given list. The syntax `${name%cpp}` produces the value for the variable name without the trailing `cpp`

*Ex*. How many times does a word `$1` occurs in a file `$2`

~~~ bash 
#!/bin/bash
x=0
for word in $(cat $2); do
	if [ $word = $1 ]; then 
		x=$((x+1))
	fi
done 
echo $x	
~~~

*Ex*. Payday is on the last Friday of the month. Compute this month's payday. 

~~~ bash 
#!/bin/bash 
report() { #inside a fn $1 on denote the *fnc's* parameters
	if [ $1 - eq 31 ]; then 
		echo "This month: the 31st"
	else 
		echo "This month: ${1}st"
	fi
}

# compute the last Friday, and pass as an arg to report  
report $(cal $1 $2 | awk '{print 6}' | egrep "[0-9]" | tail -1)

# $(cal $1 $2 ...) -> $1 = month, $2 = year (vars)
~~~

Note that the `$1` outside of report was the scripts first cmd line arg, and in report it was the fcn's cmd line argument. 

-> `$0` does not change as it is the currently running program, and the fcn is not a program 

`awk '{print 6}'` -> prints the sixth column (friday column) of the calendar (fed inout) 

command in a command -> subshell 

# Testing 
- An essential part of program development
	- Ongoing - not just test at the end 
	- Begins before you start coding
	- Test suites test *expected behaviour* 
	- Continue to test while you code 
- **Testing is not debugging - cannot debug without testing** 
- In general, testing cannot prove the corectness of a program, can only prove wrongness 
- Is not easy: 
	- No general formula 
	- psycologucal barrier - don't want to find out your program is wrong 
	- ideally developer and tester should be different (not in this course) 

***Human testing:*** 

- Humans look over code, find flaws 
- Code inspection, walkthrough 
- Can't do this on assignments (save for rubber duck debugging) 

***Machine test:***

- run program on selected inpout, check against spec 
- Can't check everything: choose test cases carefully 
- Black/white/grey box testing - no/full/some knowledge of program implementation 
- Start with **black box** testing, then supplement with white box testing 
	- Various classes of input, e.g numeric ranges, positive, negative numbers 
	- Boundaries of valid ranges (edge cases), multiple simultaneous boundaries (corner cases) 
	- Intuition/experience - guess at likely errors 
	- Extreme cases (winthin reason: ex. overflow, underflow) 
- **White box** testing: 
	- Execute all logical paths through your program 

DO NOT TEST INVALID INPUT (in this course), unless a behaviour has been prescribed 

\! If the input is invalid, and there is no specifed behaviour, then there is no such thing as correct output 

***Program testing***: is the program efficient enough?

> ***Regression testing***: ensures new changes to the program do not break old functionality

- Typically use test scripts, testing suites 
- Always add, never subtract test cases 

