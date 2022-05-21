 # Lesson 0

```
|mount %base
```

this will automatically mount a folder in your ship's dir called `base` with a copy of your data

if you add a new file, you need to re-sync your fs by running that again

list files: 

```
+ls %/mar
```

cat files:

```
+cat %/mar/xml/hoon
```

--- 


# Lesson 1

everything that happens in the dojo results in a value; 'no side-effects' of hoon code, it always deals manipulating nouns and handing them back

urbit outcomes are deterministic or functional -- the same inputs always provide the same outputs

numbers have to by decimal-separated every 3 chars

in dojo, you can use runes to perform operations
- each rune has some number of children, each of which is separated by 2+ spaces
- to add, `%- add  [2 2]`
  - `%-` says 'apply this operation to its children'
  - it's children are 'add' and a pair of numbers
   - `add` is defined in the stdlib

you can denote type in the dojo with prefixes
- starting a string with `0b` denotes as binary
  - `dojo> 0b01101`
- starting with `.` denotes decimal
- `-` is negative
- `--` is some other kind of negative

special notation to convert between types:

```
`@ub`54
```

'convert 54 to binary', called casting

### ^-  |  kethep

`^-` (kethep) takes two children; says 'i guarantee that these two children have the same type
- example: `^-  @ud  54`
- you can nest them, having a rune with another rune as a child
  - `^-  @ux  ^-  @  54`
  - this is a tree that evaluates right to left:

```
    ^-
    / \
  @ux ^-
      / \
     @   54
```

everything in hoon is built with runes, except for values

### :-  |  colhep


construct a cell with two values using `:-` (colhep):

`:-  5  6` evaluates to `[5 6]`

### =/  |  tisfas

three children to a `=/`
- a title
- a value
- expression which knows about the value

```
=/  perfect-number  28  
%-  add  
:-  perfect-number  10
```

the `~` (sig) is the null character -- represents the end of a list

### %-  |  cenhep

`dojo> %-  gth  [5 4]`

"is 5 greater than 4?"

make a decision between two hoon operations
- e.g is the password correct? is the value x greater than y?

### ?:  |  wutcol

`dojo> ?:  %.y  2  3`

takes 3; if yes, take first path, if no, take the second

```
=/  x  11          :: x=11
?:                 :: conditional:
  %- gte  [x 10]   :: if x >= 10
1                  :: return 1
0                  :: else return 0
```

"is x greater than 10? return 1 if yes, return 0 if no"

### summary so far

noun is an atom or cell
rune is a verb

#### 2 children
`%-`  apply a function (gate)

`^-`  enforce a representation (empty is @, or @ud @ub @ux @p @rs) 

`:-`  make a cell

#### 3 children

`=/`  name a variable

`?:`  make a decision 

@rs = single-precision real number

```
=/  x  2
  ?:
  %-  gth  [x  2]
    ?:
    %- lte  [x 3]
      %-  min  x  2
      ?:
        %-  lth  [x 3]
          %-  gte  [x 4]
          0
    0
0
```

# Lesson 2

the dojo will do **pretty-printing** for a lot of stuff; many of the commands you use are just aliases. there is also **irregular syntax**, which is just another way of writing the same thing

```
`@ux`110.000
```

is a shorthand for:

```
^-  @ux
^-  @
110.000
```

you can also use parentheses to call gates; eg, write `%-  add  :-  1  3` can simply be written as `(add 1 3)`

so, when you see sugar syntax, try to think of what it's an alias for

putting single-quotes around values tells urbit that the thing contained is a string -- they are called **cords**

if i want to convert this back to runic form:

```
['lorem 'ipsum' 'dolor']
```

i can construct it like this, by buidling a tuple with collus:

```
:+  'lorem'  'ipsum'  'dolor'
```

simple conditional branch:

```
=/  x  5
?:  (gth x 5)
'hello'
'goodbye'
```


# Lesson 3

whitespace works by having the main spine of the application's logic aligning with the leftmost side of the document, with increasing indentation with each branch, returning to the left as the branches terminate 

### loops

a **trap** is a point in the code that you return to after a loop, using `|-` barhep

`%=` centis reboots the current subject having made a list of changes to the values present in the code; in our case, using it for the main loop, to modify values in the subject; can accept any number of children, must terminate it with `==`

```
:: this program will increment a counter and add
:: each value to the sum of previous values, up
:: to 5
=/  counter  1   :: for counting
=/  sum  0       :: the thing i'm counting
|-               :: barhep is for traps
~&  "counter:"
~&  counter
~&  "sum:"
~&  sum
?:  (gth counter 5) :: this is my condition>
  sum               :: to leave the trap
%=  $               :: and our main loop
  counter  (add counter 1) :: add 1 to the counter
  sum  (add sum counter)   :: add current count to sum
==                   :: reboot current subject
```

`$` buc is a way of referring to the trap; this program is equivalent to a for loop

`~&` sigpam is a way of telling the interpreter a hint, not actual nock; produces a 'side-effect' but doesn't affect your code

```
:: factorial
|=  n=@ud     :: open a gate that takes an @ud
|-            :: start a trap
~&  n         :: output n each time
?:  =(n 1)    :: my exit condition for the loop, equality test
  n           :: return n
%+  mul       :: apply a gate to pair of values (next two values)
n             :: n, and
%=  $         :: main loop
  n  (dec 1)  :: n-1
==
```

### cores

a **core** is a cell pairing operations to data:  [battery payload]

**battery** is the things that can be done, the **payload** is the data to perform them on; both the input values and the subject

everything nontrivial in hoon is a core

cores have 2 kinds of values, **arms** and **legs**; arms are computations, marked by `++` or `+$`; legs are data

`|%` barcen is for building cores

`++` luslus is for building arms

```
|%  
++  add-one  :: build core called add-one
  |=  a=@ud  :: here is the gate
  ^-  @ud    :: 
  (add a 1)  :: 
++  sub-one  :: build core called sub-one
  |=  a=@ud  :: here is the gate
  ^-  @ud    :: 
  (sub a 1)  :: 
--           :: rune terminator
```

`+$` is used for building types, eg

```
|%
+$  suit  ?(%hearts %spades %clubs %diamonds)
+$  rank  ?(1 2 3 4 5 6 7 8 9 10 11 12 13)
+$  card  [sut=suit val=rank]
+$  deck  (list card)
```

```
|=  n=@ud   :: gate that accepts n
=<          :: takes add-one and n
(add-one n)
|%          :: define add-one core
++  add-one
  |=  a=@ud
  ^-  @ud
  (add a 1)
--          :: end the |%
```

alternate composition:

```
|=  n=@ud   :: gate that accepts n
=>
|%          :: define add-one core
++  add-one
  |=  a=@ud
  ^-  @ud
  (add a 1)
--          :: end the |%
(add-one n)
```

hoon isn't procedural where you just list steps, everything has to live in the code tree

### binary trees

3 ways to access values in a binary tree

- numeric addressing, useful when you know the address directly

- positional addressing, useful when we know how to navigate to something but dont know the number

- wing addressing, where you apply a name to it and address that

you can do numeric addressing like this: 

`+1:[[5 6] [7 8]]`

relative/positional addressing uses 'mark' notation, alternating like:

```
             .
          -     +
   -<  ->         +<   +>
-<- -<+ ->- ->+ +<- +<+ +>- +>+
```

applied in the same way: `-<:[[4 5] [6 7]]`

a tape is just a big binary tree of characters, so you can address invidividual letters this way

wing addressing: here is an example of a named tuple, where we can address the wings by their names:

`=data [a=[aa=[1 2] bbb=[3 4] bb=[5 6] b=[7 8]]`

`a:data`

`bbb.aa.a.data`

address from the inside out

going back to `%= $` -- if a core only has one arm, that arm is named `$`

```
%=  ^
  counter  (add counter 1)
  sum  (add sum counter)
==
```

what centis says above is: take the core that i'm in and reevaluate the buc arm with these values

irregular form for use of `%=` above: 

```
$(n (dec n), counter (add 1 counter))
```


# Lesson 4

there are 4 ways to represent text in urbit; somewhat interoperable, tend to be used in different ways

- `@t`, or **cord** -- written in 'single quotes', a single ascii/utf8 character or many written next to each other in memory; efficient for resources but not manipulation

- a **tape** is written in "double quotes" (`tape` type) -- a list of `@t` characters with pretty-printing

convert to hex (must pass through empty aura first):

`(list @ux)``(list @)`"hello"

- `@ta` is a **knot** -- used for things like url's or file system paths

- `@tas` is a **term** -- this is stuff like '%pals'

example of a mold: 

```
=elem  |=  input=@t
=<
(validate-element input)
|%
+$  element  ?(%earth %air %fire %water)
++  validate-element
  |=  incoming=@t
  %-  element  incoming
```
this takes a tape and validates it against an element type union, making sure the input matches one of the given options; call it like: `(elem 'water')`

simple type check called `sane`, where you pass it a type and a value and check it: `((sane %tas) 'hello')`, which produces `%.y` or `%.n`

there are lot of little tools for working with text, most of them are for tapes (convert back and forth if necessary); to build a tape from scratch, you can use **string interpolation**; example:

`"{<(add 5 6)>} is the answer to 5 + 6."`

**weld** is an operation that works on all lists, including tapes:

`(weld "hello" "Mars")`

**trip** converts from a cord to a tape

`(trip 'hello')`

**crip** goes from tape to cord the same way

**cass** and **cuss** convert to lowercase/uppercase

**find** will search a tape for a 'needle in a haystack':

`(find "Mars" "hello Mars!"`

we can pull values out of a tape with wings or positional addressing

`+:(find "Mars" "Hello Mars!")`

this returns `u=6` which is the face of the value; we can strip it off:

`u.+:(find "Mars" "Hello Mars!")`

you can count the length of a tape with `lent`: 

`(lent "Mars")`

**tokenizing** text means breaking it into pieces per some rule; eg breaking up a sentence into words by looking for spaces

`!,` zapcom parses a tape as hoon

```
`(list @)`['a' %b 100 ~]
```

list *always* has to end with `~`; you can also write it with the sig at the beginning: `~[1 5 21]`

the **type spear** is `-:!>`

```
-:!>(~[3 4 5])
```

it takes what is given and returns the type information

**flop** takes a list and reverses it: `(flop ~[3 4 5])`

**sort** takes a list and orders it: `(sort ~[1 5 2 4 3] lth)`

it's good practice to cast to a list explicitly so we can catch issues that might otherwise slip past

**gulf** builds a list between given values: `(gulf 1 5)`

**reap** accepts a number and something to repeat that many times: 

```
`tape`(reap 8 'A')
```

**snag** takes things out at an index: 

```
(snag 0 `(list @ud)`~[1 2 3 4 5])
```

**scag** returns everything up to a given index: `(scag 5 "Hello Mars!")`

**slag** returns the index and everything after it:  `(slag 5 "Hello Mars!")`

**roll** will combine a list: 

```
:: this will add up the contents
(roll `(list @ud)`~[1 2 3 4 5] add)
```
```
:: this will perform factorial
=fact  |=  n=@ud
(roll (gulf 1 n) mul)
```

**turn** takes a list and applies a gate or mold to it:

```
(turn `(list @ud)`~[65 66 67 68 69] fact)
`tape`(turn `(list @ud)`~[65 66 67 68 69] @t)
```

build a parser which splits a long tape into smaller tapes at single spaces (break it up into words), producing a list of tapes:

```
:: as a generator
|=  ex=tape
=/  index  0
=/  result  *(list tape)
|-
  ?:  =(index (lent ex))  result
  ::  one case is this is a regular char
  ::  one case this is a space
  ?:  =(' ' (snag index ex))
    %=  $
      index  0
      result  (weld result (scag index ex))
      ex (slag index ex)
    ==
  $(index +(index))
```


# Lesson 5

subject-oriented programming is the hoon programming paradigm; comparing to other paradigms: 

- **imperative** programming focuses on control flow

- **declarative** programming focuses on the flow of data rather than control

- **functional** is a subset of declarative programming that focuses on using mathematical functions to manage data

- **object-oriented** programming is architected around the principle of data encapsulation -- structures made of data and code that operate by calling functions from each other or passing data between each other to control things

**subject-oriented** combines aspects of object-oriented and functional programming; fuses what an expression knows about with what it's evaluating; every hoon evaluation has its own context, which is the binary tree (core) that it came from


`=data [a=[aa=[aaa=[1 2] bbb=[3 4]] bb=[5 6]] b=[7 8]]`

to address into this, you work inside-out, from the bottom of the tree up -- `aaa.aa.a.data`; hoon returns the first thing it finds given the address, face or arm; name can be shared by multiple things unlike a variable; this is a **wing**, a search path

example: `d:[d=123 d=456]` returns 123; you can 'skip' with `^`, eg `^d:`

`.` refers to the current/outermost subject:

```
=d 789
^^d:[d=123 d=456 .]
```

returns 789

**cores** have [battery payload], where battery is code and payload is data; runes for cores: `|%`, `|=`, `|-`

`(|=(a=@ud (add 1 a)) 123)` -- here you can see both battery and payload together; a `list` is a gate-building gate; eg:

```
`(list @ud`~[1 2 3]
```
this gate-building gate pattern is used all the time, eg `add` and `mul` are gates built by the arms

the `add-10` generator in the notes creates a bunch of arms to add/mul/sub/div by 10; when you call one, hoon is looking up the gate and building it at that moment, slotting in the payload:

```
=c |%
++  add-10
  |=  a=@ud
  (add a 10)
++  sub-10
  |=  a=@ud
  (sub a 10)
++  mul-10
  |=  a=@ud
  (mul a 10)
++  div-10
  |=  a=@ud
  (div a 10)
++  mul-20
  |=  a=@ud
  (add (mul-10 a) (mul-10 a))
--
```

we can also address into cells, including addressing into arms; this returns nock though, so not always super useful; if you just enter `dec` into dojo you get this:

`<1.hkg [a=@ <46.hgz 1.pnw %140>]>`

this is basically the numbers of arms in a core, the aura it takes, and like a hash or something; %140 is the hoon version

address into this like `+6:dec` and you get a bunch of nock

a **payload** is made up of a **sample** and **context**: 

`[battery [sample context]]`

the context is the outer subject that you're in; in the dojo, that means arvo, hoon, stdlib etc; a naked generator doesn't know about the subject's full context, it just has the stdlib

`.` refers to current subject; `..` resolves into the given subject (`..add`); sometimes you need to modify parts of a core to get desired behavior; 

ex: **roll** takes a list and applies an operation to each value in it:

`(roll `(list @ud)`~[10 12 14 16 18] mul)`

if we look inside at the tail: `+:mul` we can see it has a default sample: `[[a=1 b=1] <46.hgz 1.pnw %140>]`

`mul:rs` is for multiplying single-precision numbers; we can look at the default value with `+6:mul:rs`: `[a=.0 b=.0]`

we can manually adjust the gate like so:

`=mmul |=([a=_.1 b=_.1] (mul:rs a b))`

now it has default values of .1: 

```
(roll `(list @rs)`~[.10 .12 .14 .16 .18] mmul)
```

and it returns a fractional value

`|%` builds a generic core with arms
`|=`  builds a core with `$` arm and evalutes `$`
`|-` builds a core with $ arm, no sample, and evaluate

a trap evaluates immediately, a gate doesn't evaluate until you add a `%=`

new rune is `|_`, which builds a core with sample and arms with samples, called a **door**; a function-building function, or a gate-building core

they're very good at building and storing data structurs and state machines

if we want to calculate `y = a x^2 + b x + c`, we need to keep track of both variables and parameters, which are different kinds of things; here is a gate of this function:

```
=poly-gate |=  [x=@ud a=@ud b=@ud c=@ud]
(add (add (mul a (mul x x)) (mul b x)) c)
```

we can call it like `(poly-gate 2 5 4 3)`, 3 variables and one parameter; this is inconvenient, so we can wrap it in another gate with pinned values:

```
=wrapped-gate |=  [x=@ud]
=/  a  5
=/  b  4
=/  c  3
(poly-gate x a b c)
```

this is a core that accepts a sample:

```
=poly |_ [a=@ud b=@ud c=@ud]
  ++  quad
    |=  x=@ud
    (add (add (mul a) (mul x x) (mul b x)) c)
  --
```

the `%~` censig rune can call both like this:

`%~  quad  poly [5 4 3]`

which says 'pull the arm quad inside of the door poly with the sample [5 4 3]; this returns a gate built to these specs

the easy way to write the above is `~(quad poly [5 4 3])`

remember that this results in a gate -- calling a gate-building door to build a gate; then we can give this a face: `=q  ~(quad poly [5 4 3])` and call it with `(q 2)` to call the quadratic function

`|:` barcol allows you to build a custom sample of door (more in notes)

`|^` barket allows you to build a core with `$` arm and more arms; computes immediately like a trap

`|.` build a core with arm and no sample

```
=volume-of-cylinder |^                 :: barket
(mul:rs (area-of-circle .2.0) height)  :: this is the $ arm
++  area-of-circle
  |=  r=@rs
  (mul:rs pi r)
++  pi  .3.1415926                     :: subsidiary arms
++  height  .10.0
--
```
here barket; useful to structure everything in arms for a conceptually complex operation

`|.` bardot useful to build a core with a `$` arm and no sample; so you can just put something in the `$` arm, pull it out, look at it etc; just something you can carry around with a `$` inside of it

all of these bar runes kind of construct a matrix of all possible things you can do to build cores; this is very elegant once it clicks for you

a **gate** is a door with only on arm, `$`; but when you invoke it that default arm's expression is handed the sample and evaluated; very similar to a trap; the diff is that a gate has a sample and isn't evaluated immediately

**paths** on mars are ways to organize data and allow you to point to a resource; a **desk** is a discrete collection of data on a particular ship; it's a noun, it's a tree, like everything else, but ultimately it's a mold; kind of like a git branch; when we access something in clay, we need to provide 3 pieces of information:

- @p
- desk
- system time

you can see your current position with `%` in the dojo; this triplet of data is called the **beak**, a list of `%tas` terms; so you can path to something like `%/desk/bill`; clay is a global filesystem, meaning you can refer to any file on any desk at any time (though most of these do not exist, like street addresses you could come up with)

`+cat %/desk/bill` will resolve a list of stuff in the desk; sometimes a path will have a number in it, which is the version; you can refer to file versions via timestamps or you can do it by version: `+ls /~zod/base/2`


# Lesson 6

`!:` rune can be used for debugging if you include it a the top of your generator file, spells out where things are breaking

3 main kinds of errors:
- general syntax errors
- mismatched rune daughter syntax errors (eg too few daughters)
- type errors (need/have)

we are focusing on the type issues; to solve these, use `^-`/`^+`
- `^-` says 'enforce with this aura'
- `^+` says 'enforce with this example' (more useful with complex data structures)

**assertions** -- for example, testing like:
- `?:  =(t.list ~)  length` to see if the tail of something is null (ie a list)
- `?~` wutsig branches on null

when typechecked, hoon checks if it is one of two things: a `~`-terminated collection of atoms, or an empty list (a `~`). if we want to address into it like `i.whatever`, we need to prove to hoon that it's a **lest**, a non-empty list

let's test a list: 

```
=/  my=(list @)  ~[1 2 3]
?~  my  !!                 :: this asserts that it's non-empty
i.my
```

alternately:

```
=/  my=(list @)  ~[1 2 3]
?<  =(my ~)               :: wutgal is a negative assertion
  my
```

(wutgal is used to force a crash when a condition doesn't yield 'no', `?>` is for 'yes'); these are usually used to solve `-find.i.whatever` type errors but not frequently

anything identified as a **spec** is using structure-mode, which is used to construct things a little differently

irregular vs long-form/structure-mode  way of building:

```
a=[@ud @ud])
$+  m  a=$:(@ud @ud)
```

enforcing types:

`+$  url  @t`
`+$  rank  ?(%galaxy %star %planet)`


### maps

a **map** is a pattern from a key to a value (a dictionary)

`map` is a mold or type of a value; it's the mold that defines itself, like `list`

```
(list @ud)
(map @tas @ux)
```

`=colors (my ~[[%red 0xed.0a3f] [%yellow 0xfb.e870] [%green 0x1.a638]])`

this means it has a door that builds the arms as we need them; 
`put` is gate located in `by` door; let's try inserting new cell of color/value:

```
(~(put by colors) [%orange 0xff.8833])
```

this won't work, because %tas will build their own type union when you use them; if we typespear it (`-:!>(colors)`) we can see hoon thinks `colors` is a union of the specific terms previously defined and a @ux, not a %tas type. we can resolve this when we define the map by casting:

```
=colors `(map @tas @ux)`(my ~[[%red 0xed.0a3f] [%yellow 0xfb.e870] [%green 0x1.a638]])
```

### map operations

a map allows us to distinguish between an empty result and a 0 result; if we ask for something that doesn't exist, it returns sig

`(~(get by colors) %teal)` retrieve value as a unit

`(~(gut by colors) %teal 0x0)` allows us to specify a failure/default (eg returning black here)

`(~(got by color) %teal)` is like get but crashes if the value isn't present

`~(key by colors)` lets us look at all the keys in the map

`~(tap by colors)` shows us both key & value

this gate will let us snip stuff out of a value:

`=red |=(a=@ux ^-(@ux (cut 2 [4 2] a)))`

we can apply it to the whole map:

`(~run by colors) red)`

### sets

a **set** is built with `sy` (like a map with `my`); recognizable by use of curly braces

`=primes (sy ~[2 3 5 7 11 13])`

`(~(put in primes) 17)` 

`(~(del in primes) 17)`

`~(tap in primes)` returns it as a list

`(sort ~(tap in primes) lth)` applies list operations to it

`(~(has in primes) 15)` boolean test

`(~(run in primes) dec)` lets us run eg math operations on it

a **jar** is a map of lists: `(jar @t @t)` takes a cord key and maps to list of cords; eg:

- 'Mazda' -> ~['RX-8' 'Miata' 'MX']
- 'Dodge' -> ~['Viper' 'Challenger']

a **jug** is a map of sets

### structures and their doors reference

| type | syntax         |door|
| ---  | ---            |---|
|list  |`(list @ud)`    ||
|map   |`(map @tas @ux)`|by|
|set   |`(set @ud)`     |in|
|jar   |`(jar @t @t)`   |ja|
|jug   |`()`            |ju|


### misc caesar notes

`cass` converts to lowercase

useful debugging tool is to print vars, like `~&  "rotation: {<[p q]>}"`

`=|` is for pinning the bunt: `=|  value=@ud` same thing as `=/  value  *@ud`

`~|` sigbar -- sig runes are runtime hints; sigbar is a tracing message, which it will print if something goes wrong -- ex: `~|  %uneven-lengths  !!`


# Lesson 7

`gall` is the userspace vane

`hoon.hoon` is language interpreter stuff; most of this lesson's video is going through it and looking at how stuff works

in good hoon, the 'heavier' branch should be lower in the file

`?-` wuthep and `?+` wutlus are 'switch statements' -- choose between possibilities, like a type union; must handle every case and no case must be unreachable

?^ wutket asserts cell, ?@ wutpat asserts atom

logical operators: `?&` wutpam AND `?|` wutbar OR `?!` wutzap NOT

`;:` miccol, irregular `:()` like `:(add 3 4 5)`, changes the 'arity' of a gate -- change binary gate to n-ary

**currying** is to fix a parameter in the sample; eg i want to run a multiplication operation that has a value baked in; right curry (`curr`) or left curry (`cury`) to bind to head or tail: `=one (curr full [4 3 2])`

we can chain gates together so that the result of one applies to the next: `(dec (@ux 20))`

we can use `cork` to chain gates together: `((cork dec @ux) 20)`

`cork` and `corl` chain gates together

`sun:rs` converts regular atom to its single-precision fp equivalent: `(sun:rs 1)`

`turn` takes a list and applies to each item: `(turn (gulf 1 5) add)`

`roll` applies an operation to each item in a list: `(roll (gulf 1 5) add)` ("right fold")

`reel` is the same but moves left-to-right across list (left fold)

### formatted text

`tank` (plural `tang`) -- a formatted print tree; just a cord, or a cell where the head label is the kind of thing it is (leaf/palm/rose):

```
::  $tank: formatted print tree
::
::    just a cord, or
::    %leaf: just a tape
::    %palm: backstep list
::           flat-mid, open, flat-open, flat-close
::    %rose: flat list
::           flat-mid, open, close
```

`~&` sucks for printing output for a user so this is what you use instead

`~(ram re leaf+"hello mars")`

`~(ram re [%rose [" " "$" "$"] leaf+"foo" leaf+"bar" leaf+"baz" ~])`


# Lesson 8: managing state

a **naked generator** (all generators up till now) is *just a gate*

a **%say generator** doesn't have the structure of a gate; a cell of `[%metadata %code]`

`ls.hoon`:

```
::  LiSt directory subnodes
::
::::  /hoon/ls/gen
  ::
/?    310
/+    show-dir
::
::::
  ::
~&  %
:-  %say                                                  :: row A 
|=  [^ [arg=path ~] vane=?(%g %c)]                        :: row B
=+  lon=.^(arch (cat 3 vane %y) arg)                      :: row C
tang+[?~(dir.lon leaf+"~" (show-dir vane arg dir.lon))]~  :: row D
```

- row B, `^` by itself is the bunt of a cell

- row D, `tang+` (tang is formatted print tree, plural), `+` in front of a cell is an irregular form to build a cell where the text before `+` is the metadata tag

say generator is a tag with `%say`, and hands back a mark and data; the tail is a **cask**, a pair of `[mark data]`; so `[%metadata [mark data]]`

**marks** are like molds, used for data validation

simple %say generator:

```
:-  %say
|=  *
:-  %noun
(add 40 2)
```

this allows us to execute this with zero or more arguments; the sample of this gate is `*`, which says 'throw away whatever you get'

say generator takes a cell of 3 cells: 

`[[now=@da eny=@uvJ bec=beak] [list of unnamed arugments] [list of named arguments]]`

unnamed arguments are required, named are optional; this is what we were stubbing out with `*` above

magic 8 ball say generator:

```
!:
:-  %say
|=  [[* eny=@uvJ *] *]
:-  %noun
^-  tape
=/  answers=(list tape)
  :~  "it is certain"
      "it is decidedly so"
    [etc]
  ==
=/  rng  ~(. og eny)
=/  val  (rad:rng (lent answers))
(snag val answers)
```

`(snag val answers)` is randomly selecting the item out of the list -- `rad:rng` uses the random number arm on the length of the list `answers`
(rad:rng 100)
`~(. og eny)` pulls an arm in a door with the value 'entropy'; `.` is the subject, `og` is the core that provides rng services

```
:-  %say
|=  [[now=@da eny=@uvJ bec=beak] [n=@ud ~] [bet=@ud ~]]
:-  %noun
[(~(rad og eny) n) bet]
```

`@uv` is a base32 representation, the `J` (or a capital on the end of an aura) is used to define the bit-width of the value, in this case 512bit number

this has two arguments: `n` which is required, `bet` which is optional

this generator will 'roll a dice' with the given value as the max: `+dice 5` will roll a number 0-5; the `bet` argument is optional, `+dice 5, =bet 200`; will add the bet value to the tail of the output; this comma syntax is necessary for the optional argument

this toy program demonstrates that you can write code with values that are unknown at the time it's executed and deal with deferred computation, which we'll refer to as state

### state management runes

- `=.` tisdot changes a leg in the subject
- `=^` tisket changes a leg in the tail of the subject, then evaluates against it
- `=*` tistar defers an expression (gall agents)
- `=~` tissig composes expressions together
- `;<` micgal sequences two computations (used in threads, good for asynchronous calls that might fail)
- `;~` micsig produces a pipeline (pipe gate output into another)

micsig and tisket are going to be our focus here

let's build an rng: 

```
=/  rng  ~(. og eny)
[(rad:rng 100) (rad:rng 100)]
```

this will keep returning the same result; it hasn't modified itself after it's first run; this is what `=^` tisket is for:

```
=/  rng  ~(. og eny)
  =^  r1  rng  (rads:rng 100)
  =^  r2  rng  (rads:rng 100)
[r1 r2]
```

call with `[r1 r2]` -- (`rads` returns the random number and the changed subsequent state)

we're going to now look at the hoon parser, which exists to read a utf8/ascii text file, convert it into an abstract syntax tree and execute it 

### tape parsing

- a tape is a string to be parsed
- `hair` is the position the parser is at `[col line]`
- `nail` is parser input `[hair tape]` (where does it start)
- `edge` is parser output `[hair [~ [hair nail]]]` (where did it get)
- `rule` = parser

to parse is to use a rule on a nail to yield an edge

`scan` is a gate that parses a tape or crashes; an example parser:

`(scan "after" (star (shim 'a' 'z')))`

`shim 'a' 'z'` matches a character in a range; "after" is the sample of the gate

#### parsing rules

-`shim` matches character in range

`((shim 'a' 'z') [[1 1] "a"])`

`[1 1]` is the nail for this gate

- `star` combinator for any number of occurences of the next rule

- `just`  matches specified sample

`((just 'a') [[1 1] "abc"])`

- `jest` will return the rest of a string after the matching sample

`((jest 'abc') [[1 1] "abcdef"])`

- `pose` will allow us to match multiple things, more than one rule

`(;~(pose (jest 'abc') (jest 'def')) [[1 1] "abcdef"])`

scan is pretty easy to use:

`(scan "a" (just 'a'))`

`(scan "aaaaa" (star (just 'a')))`

two issues with the types of rules so far:

1. we only get back raw @t -- this is addressed with `++cook`

`((cook @tas (just 'a')) [[1 1] "abc"])` -- this will cast output to @tas

this will eat the first character and cast output to @tas:

```
((cook |=(a=@ `@t`+(a)) (just 'a')) [[1 1]] "abc"])
```

match with shim: 

```
((cook |=(a=@ `@t`+(a)) (shim 'a' 'z')) [[1 1] "abc"])
```

2. limited to number of gates for number of characters, fix with `++knee`



# Lesson 9: producing code

debugging, testing & code distribution

by the time you can satisfy the static type checker, you've eliminated whole categories of software problems

- a **failure** is observable incorrectness
- a **fault** is discrepancy in code
- an **error** is mistake in judgement or implementation
  - **manifest error** is known
  - **latent error** will arise under as-yet-unknown conditions
  - **masked errors** are concealed by another aspect of the program

`nest-fail` is most common when something expects an atom or a cell and receives the other

`mint-nice` errors usually occur with type checking; this is why you have to cast some things through an empty aura

`mint-vain` happens when there's a branch that can never be evaluated, `mint-lost` means there's something that isn't dealt with

`fish-loop` means it can't check for recursive mold definitions, like in lists

`generator-build-fail` can happen from bad structure, spacing wrong for runes, wrong number of children

`bail:meme` is OOM runtime error

`find.$.<wing>` means you're trying to use a non-gate as a gate, can't find the battery or buc arm inside of it

`!:` zapcol tells you where the expression that caused trouble arose in your stack trace; works everywhere, drop it in wherever so you can tell what's going wrong

### unit tests

testing is designed to manifest failures of code so that faults/errors can be identified and corrected; the extreme version of this is 'test-driven development', which is designing your code ahead of time as a series of tests, and once your code meets those tests it's ready

a **unit test** interrogates individual arms in your code

- positive unit tests describe something that should happen
- negative unit tests test something that should not happen

a **desk** is a discrete collection of data, libraries, unit codes etc stacked on top of your system; use `%` to see current desk info

tests always have this structure:

```
++  test-is-root
  ;:  weld
    %+  expect-eq
    !>  %.y
    !>  (is-root:ver %update-0)
    ::
    %+  expect-eq  
    !>  %.y
    !>  (is-root:ver %update)
    ::
    %+  expect-eq  
    !>  %.n
    !>  (is-root:ver %not-update-0)
  ==
::
```

3 different tests above; `;:` lets us use more arguments than normal, weld lets us join the result of two operations together; `!>` creates a vase of a value with its type info and value; `!>  %.y` is the expected value, the second `!>` is the actual value, pulls the arm and makes sure it does what it should do

if someone finds a bug in your code, you should add another test that tests for it

### software distribution

desks are used for software distribution 

you can invoke a generator on a particular desk like `+landscape!tally`; you can show files on one like `+ls /=lansdcape=`, or print them like `+cat /=landscape=/desk/docket-0`

to create a desk: 

- `|merge %sample our %base, =gem %init` -- the second one is the source, like `git branch`, the `=gem %init` part means you're initializing it from scratch; 
- `|mount %sample`
- replace the contents of `desk.bill` with a single `~` so it's not junked up with agents
- `|commit %sample`
- `|install our %sample`
- `|public %sample` (alternately `:treaty|publish %sample` but this requires a docket file)

you need too build a desk.docket-0 file, docket.hoon in /lib, sur/docket.hoon, and a docket mark file in /mar:

https://github.com/urbit/urbit/blob/master/pkg/garden/desk.docket-0

https://github.com/urbit/urbit/blob/master/pkg/garden-dev/lib/docket.hoon

https://github.com/urbit/urbit/blob/master/pkg/garden-dev/sur/docket.hoon

https://github.com/urbit/urbit/blob/master/pkg/garden-dev/mar/docket-0.hoon

