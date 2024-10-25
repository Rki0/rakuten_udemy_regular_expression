# Regular Expressions (Regex)

## The Basic Set

### `.` (Wildcard Symbol)

```
<Input>

fooabar
fooxbar
baryfoo
foobar
fooxybar
foocbar
-------------
<Regex>

foo.bar
-------------
<Result>

fooabar
fooxbar
foocbar

```

### `*` (Asterisk)

> Zero or more occurences of wildcard, <br />
> which means zero or more occurrences of any character.

```
<Input>

foobar
barfoo
fooabcbar
foobxcbar
barcbyfoo
foozbar
barafoo
barabfoo
-------------
<Regex>

foo*bar

-------------
<Result>

foobar
fooabcbar
foobxcbar
foozbar
```

### `\s` (Whitespaces)

> `\s` represnets whitespace. <br />`\s*` represents zero or more occurrences of whitespace.

```
<Input>

fooxxxbar
foo   bar
fooxbar
fooxxbar
foo bar
foo      bar
foobar
fooyyybar
-------------
<Regex>

foo\s*bar

-------------
<Result>

foo   bar
foo bar
foo      bar
foobar
```

### `[]` (Character Classes)

> `[abc]` - Character class. <br />
> One of the characters inside the square brackets - a, b or c

```
<Input>

foo
moo
coo
doo
poo
loo
boo
hoo
-------------
<Regex>

[fcl]oo

-------------
<Result>

foo
coo
loo
```

> `[^abc]` - Any character EXCEPT any of the ones inside the square brackets, in a single position.

```
<Input>

foo
moo
coo
doo
poo
loo
boo
hoo
-------------
<Regex>

[fcdplb]oo

or

[^mh]oo

-------------
<Result>

foo
coo
doo
poo
loo
boo
```

> `[a-c]` - One of the characters falling in the range given in square brackets - a,b,c

```
<Input>

joo
boo
koo
loo
woo
moo
zoo
coo
-------------
<Regex>

[j-m]oo
-------------
<Result>

joo
koo
loo
moo
```

> `[a-cx]` - One of the characters falling in the range OR any of the other choices given in square brackets - a, b, c, x

```
<Input>

joo
boo
koo
loo
woo
moo
zoo
coo
-------------
<Regex>

[j-mz]oo
-------------
<Result>

joo
koo
loo
moo
zoo
```

> `[a-cA-Cx]` - One of the characters falling in one of the ranges OR any of the other choices given in square brackets - a, b, c, A, B, C, x

```
<Input>

joo
boo
Koo
Loo
woo
moo
zoo
coo
-------------
<Regex>

[j-mJ-Mz]oo
-------------
<Result>

joo
Koo
Loo
moo
zoo
```

### `^$*.[()\` (Escaping With Backslash)

> Following characters should be escaped with a backslash as these characters have special meaning otherwise: `^$*.[()\`

```
<Input>

xxx.yy
xx.yyyy
x.yy
xy
xxyy
yyxx
yx
yxxx
-------------
<Regex>

x*\.y*
-------------
<Result>

xxx.yy
xx.yyyy
x.yy
```

> If a `.` is inside square brackets, it need NOT be escaped.

```
<Input>

x#y
x:y
x.y
x&y
x%y
-------------
<Regex>

x[#:.]y
-------------
<Result>

x#y
x:y
x.y
```

> If any of the characters `^,-` appear inside square brackets, it needs to be escaped with a backslash as these two characters have special meaning inside square brackets.

```
<Input>

x#y
x:y
x^y
x&y
x%y
-------------
<Regex>

x[#:\^]y
-------------
<Result>

x#y
x:y
x^y
```

> The backslash itself should always be escaped with a backslash, irrespective of its position within the regex: `\\`

```
<Input>

x#y
x\y
x^y
x&y
x%y
-------------
<Regex>

x[#\\\^]y
-------------
<Result>

x#y
x\y
x^y
```

### `^`, `$` (Anchors)

> `^` is a placeholder that signifies beginning of a line.<br />
> The interpretation of `^` differs within square brackets and outside of it.<br />
> Inside square brackets, `^` stands for negation.<br />
> Outside, it is a placeholder for beginning of line.

```
<Input>

foo bar baz
bar foo baz
baz foo bar
bar baz foo
foo baz bar
baz bar foo
-------------
<Regex>

^foo.*
-------------
<Result>

foo bar baz
foo baz bar
```

> `$` is a placeholder that signifies end of a line.

```
<Input>

foo bar baz
bar foo baz
baz foo bar
bar baz foo
foo baz bar
baz bar foo
-------------
<Regex>

.*bar$
-------------
<Result>

baz foo bar
foo baz bar
```

```
<Input>

foo
foo bar
baz foo
foo bar baz
baz bar foo
-------------
<Regex>

^foo$
-------------
<Result>

foo
```

## The Extended Set

### `{}` (Curly Braces Repeater)

> `a{m}` represents exactly "m" repetitions of whatever immediately precedes this, i.e. "a"

```
<Input>

834
519
4874
5
89
45687
25
645
-------------
<Regex>

^[0-9][0-9][0-9]$

or

^[0-9]{3}$
-------------
<Result>

834
519
645
```

> `a{m,n}` represents at least "m" and at most "n" repetitions of whatever immediately precedes this, i.e. "a"

```
<Input>

lion
tiger
leopard
fox
kangaroo
bat
mouse
cuckoo
deer
-------------
<Regex>

^[a-z]{4}$
^[a-z]{5}$
^[a-z]{6}$

or

^[a-z]{4,6}$
-------------
<Result>

lion
tiger
mouse
cuckoo
deer
```

### `{m,}`, `{,n}` (Single Ended Curly Braces Repeater)

> Parenthesis is used to group and treat as a single entity.<br /> > `{m,}` represents at least "m" repetitions of whatever immediately precedes this.

```
<Input>

ha
hahahahaha
hahaha
hahahaha
haha

hahahahahaha
hahahahahahahaha
hahahahahahahahaha
-------------
<Regex>

ha{4}
ha{5}
ha{6}
ha{8}
ha{9}

or

(ha){4,}
-------------
<Result>

hahahahaha
hahahaha
hahahahahaha
hahahahahahahaha
hahahahahahahahaha
```

> Parenthesis is used to group and treat as a single entity.<br /> > `{,n}` represents at most "n" repetitions of whatever immediately precedes this.

```
<Input>

ha
haha
hahahahaha
hahahaha
hahaha
hahahahahahaha
hahahahahaha
-------------
<Regex>

^(ha){,2}$
-------------
<Result>

ha
haha
```

### `+` (The Plus Repeater)

> `a+` - One or more occurrences of "a" (The Character just preceding the plus symbol).

```
<Input>

fooaaaabar
fooabar
foobar
fooaabar
fooxxxbar
fooxbar
-------------
<Regex>

fooa+bar
-------------
<Result>

fooaaaabar
fooabar
fooaabar
```

### `?` (The Question Mark Binary)

> `a?` - Zero or one occurrences of "a"(The character just preceding the question mark).

```
<Input>

https://website
http://website
httpss://website
httpx://website
httpxx://website
-------------
<Regex>

https?://website
-------------
<Result>

https://website
http://website
```

### `(a|b)` (Making Choices With Pipe)

> `(a|b)` represents either a or b, where a and b can be multi-character strings

```
<Input>

sapwood
rosewood
logwood
teakwood
plywood
redwood
-------------
<Regex>

(log|ply)wood
-------------
<Result>

logwood
plywood
```

## Find and Replace with Capture Groups

### The Monitor Resolutions Problem

```
<Target>

1280x720
1920x1080
1600x900
1280x1024
800x600
1024x768
-------------
<Grouping Regex>

([0-9]+)x([0-9]+)

=== Group1xGroup2
-------------
<Regex>

\1 pix by \2 pix
-------------
<Result>

1280 pix by 720 pix
1920 pix by 1080 pix
...
```

### The Fist Name Last Name Problem

```
<Target>

John Wallace
Steve King
Martin Cook
Adam Smith
Irene Peter
Alice Johnson
-------------
<Grouping Regex>

([a-zA-Z]+)\s([a-zA-Z]+)

=== Group1\sGroup2
-------------
<Regex>

\2, \1
-------------
<Result>

Wallace, John
King, Steve
...
```

### The Clock Time Problem

```
<Target>

7:32
6:12
12:23
1:23
11:33
4:21
-------------
<Grouping Regex>

([0-9]{1,2}):([0-9]{2})

=== Group1:Group2
-------------
<Regex>

\2 mins past \1
-------------
<Result>

32 mins past 7
12 mins past 6
...
```

### The Phone Number Problem

```
<Target>

914.582.3013
873.334.2589
521.589.3147
625.235.3698
895.568.2145
745.256.3369
-------------
<Grouping Regex>

[0-9]{3}\.[0-9]{3}\.([0-9]{4})

=== [0-9]{3}\.[0-9]{3}\.Group1
-------------
<Regex>

xxx.xxx.\1
-------------
<Result>

xxx.xxx.3013
xxx.xxx.2589
...
```

### The Data Problem

```
<Target>

Jan 5th 1987
Dec 26th 2010
Mar 2nd 1923
Oct 1st 2008
Aug 3rd 2009
Jun 10th 2001
-------------
<Grouping Regex>

([a-zA-Z]{3})\s([0-9]{1,2})[a-z]{2}\s[0-9]{2}([0-9]{2})

=== Group1\sGroup2[a-z]{2}\s[0-9]{2}Group3
-------------
<Regex>

\2-\1-\3
-------------
<Result>

5-Jan-87
26-Dec-10
...
```

### Another Phone Number Problem

```
<Target>

(914).582.3013
(873).334.2589
(521).589.3147
(625).235.3698
(895).568.2145
(745).256.3369
-------------
<Grouping Regex>

\(([0-9]{3})\)(\.[0-9]{3}\.[0-9]{4})

=== \(Group1\)Group2
-------------
<Regex>

\1\2
-------------
<Result>

914.582.3031
873.334.2589
...
```

## Challenges

### The Email Challenge

```
<Input>

bob.123@gmail.com
alice-personal@yahoo.in
admin@cloud.guru
tom_business@amazon.ca
george@yahoo
robert_at_gmail.com
steve austin@gmail.com
-------------
<Regex>

^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$
-------------
<Result>

bob.123@gmail.com
alice-personal@yahoo.in
admin@cloud.guru
tom_business@amazon.ca
```
