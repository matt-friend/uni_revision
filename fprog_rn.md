# Functional Programming 2018/19

##### Lecture 1

Functional Programming is about programming with values rather than statements. It encourages code that is:

	* concise
	* precise
	* reasonable

Here is an example of code in Haskell:

```haskell
fib :: Integer -> Integer
fib 0 = 1
fib 1 = 1
fib n = fib(n-1) + fib (n-2)
```

To understand this code, we evaluate it:

```haskell
	fib 2
= 	{def fib}
	fib (2-1) + fib (2-2)
=	{def '-'}
	fib 1 + fib 0
=	{def fib}
	1 + 1
=	{def '+'}
	2
```

---

##### Lecture 2

### Plain Old Datatypes

Plain old datatypes (PODs) are the building blocks of many values

```haskell
5 :: Integer
``` 
This code expresses that 5 is a value from the set Integer  
The left part of the expression is the value, and the right side the type

Numbers in haskell are overloaded so:

```haskell
5 :: Int	5 :: Float	5 :: Double
```

Other values are:

```haskell
True :: Bool	False :: Bool
'a' :: Char	'b' :: Char
"abc" :: String	"a" :: String
```

The smallest type is void. It corresponds to a type for the empty set. There are no values, except for |_

|_ is pronounce "bottom". It exptresses an undefined value. It is written:

```haskell
undefined :: a
```

The type with one element is called "unit". In Haskell, we express th evalue of unit as '()'. The type is also written as '()'

```haskell
> () :: ()
```

One thing we can do is add types together. We'll see this in a moment.

We can define a type with 7 elements in it, one for each day of the week as follows:

```haskell
data Day = Monday
	|Tuseday |Wednesday |Thursday |Friday |Saturday |Sunday 
```

This introduces "constructors" such as Monday, Wednesday with type Day:

```haskell
Monday :: Day
Wednesday :: Day
```

We can make a type with 9 values by adding the type Bool to the type Day. This is achieved with Either:

```haskell
data Either a b = Left a | Right b
```

This has introduced two constructors:

```haskell
Left :: a -> Either a b
Right :: b -> Either a b
```

Together with Bool and Day, we have  introduced Either Bool Day which is a type with 9 values. They ar ethe following:

```haskell
Left True :: Either Bool Day
Left False :: Either Bool Day
Right Monday :: Either Bool Day
...
Right Sunday :: Either Bool Day
```

Pairs correspond to multiplications

```haskell
data (a,b) = (a,b)
```

So this gives us:

```haskell
(True, Monday) :: (Bool, Day)
(True, Tuesday) :: (Bool, Day)
...
(False, Sunday) :: (Bool, Day)  14 = 2 x 7
```
### Functions

You will be familiar with functions in mathematics such as f(x) = x<sup>2</sup>

In haskell we might write this as follows:

```haskell
square :: Int -> Int
square x = x * x
```

A function takes input values to output values. Every function in Haskell takes exactly one input type to one output type.

The type of 'square' is 'Int -> Int'. The type of 'square 5' is just 'Int'.

Here is another function:

```haskell
two :: Int -> Int
two x = 2
```

This returns 2 no matter the input. e.g. two 5 = 2, two |\_ = 2

---

##### Lecture 3

We can combine functions by applying one followed by the other.

e.g.

```haskell
	square (two 5)		|		two (square 5)
= 	{def square}		|	=	{def two}
	(two 5) * (two 5)	|		2
= 	{def two}		|
	2 * 2			|
= 	{def '*'}		|
	4			|
```

We could combine these fuctions into one manually:

```haskell
squareTwo :: Int -> Int
squareTwo x = square (two x)
```

or 

```haskell
twoSquare :: Int -> Int
twoSquare x = two (square x)
```

Doing this is painful. We need a way to compose functions with an operation:

```haskell
(.) :: (b -> c) -> (a -> b) -> (a -> c)
(g.f) x = g (f x)
```

This '.' is called a "function composition". You write it with a full stop.

### Evaluation

The order of evaluation for functions is very important. There are two main strategies:
	* normal evaluation (**lazy** evaluation) - this evaluates the outer most first, before moving inwards
	* applicative evaluation (**eager** evaluation) - this evaluates inner most (parameters) first, then assembles outwards
e.g.

```haskell
Normal / Lazy			 Applicative / Eager

	square (two 5)		|		square (two 5)
= 	{def square}		|	=	{def two}
	(two 5) * (two 5)	|		square 2
= 	{def two}		| 	=	{def square}
	2 * 2			| 		2 * 2
= 	{def '*'}		| 	=	{def '*'}
	4			|		4
```

Which strategy is better? Eager appears to generate results faster. However, there can be problems. Consider the following:

```haskell
infinity :: Int
infinity = infinity + 1
```

What is the result of 
```haskell
square (two infinity) ?
```
The calculation for this in a lazy setting is similar to before:

```haskell
LAZY
	square (two infinity)
=	{def square}
	(two infinity) * (two infinity)
= 	{def two}
	2 * 2
= 	{def '*'}
	4
```

This terminates with an answer. However:

```haskell
EAGER
	square (two infinity)
= 	{def infinity}
	square (two (infinity + 1))
= 	{def infinity}
	square (two ((infinity + 1) + 1))
= 	...
```

CTRL - C to quit execution

### The Church - Rosser Theorem

This theorem tells us important facts about evaluation strategies:
	1. If evaluation of a term terminates then normal evaluation will terminate.
	2. If two evaluation strategies terminate for a term then they agree on the result.

### Currying

In Haskell it is possible to take functions as input, and to produce functions as output.

Consider two functions expressing addition:

```haskell
add :: (Int, Int) -> Int
add (x, y) = x + y

plus :: Int -> (Int -> Int)
plus x y = x + y
```

How does plus work? Consider plus 7

```haskell
plus 7 :: Int -> Int 
plus 7 y = 7 + y
```

To use this, we might write:

```haskell
plus 7 3 
	= 10
```

---

##### lecture 4

Currying allows us to convert from fucntions that take pairs as input to functions that produce functoins as output. 

For example, currying allows us to take a functoin like add, and produce a functoin like plus. this works for any function with the type:

```haskell
	(a,b) -> c
```

The function curry is defined as follows: 

```haskell
curry :: ((a,b) -> c) -> (a -> (b -> c))
curry f x y = f(x,y)

f :: (a,b) -> c
x :: a
y :: b
f(x,y) :: c
```

For example, 'curry add' behaves just like 'plus'

```haskell
uncurry :: (a -> (b -> c)) -> ((a,b) -> c)
uncurry f(x,y) = f x y
```

For example, 'uncurry plus' behaves just like 'add'

### Lambda Abstraction

In mathematics we are used to writing with operations and manipulating them with respect to equations. For instance:


x + y = z


==

x = z - y

This achieves two things:
	1. we isolate x
	2. we make use of '-' to negate the '+'

We can use lambda abstraction to achieve something similar for the arguments to functions.

How could we isolate f in the following?

f x = x<sup>2</sup>

We want to push x to the other side.

We can proceed by introducing notation that represents the fact that the 'x' has been moved:

f x = x<sup>2</sup>

==

f = (lambda) x -> x<sup>2</sup>

We have introduced the notation "(lambda) x -> e" where x is the variable that appears somewhere in e.

Some alternative notations might help to understand this:

(lambda) x -> x<sup>2</sup>

could be written as

x |-> x<sup>2</sup>

This says that x maps to x<sup>2</sup>

In a computer we write (lambda) x -> e as \x -> e.

The type of a lambda abstraction is determined by the type of the input and the type of the output.

So if we know 

	x :: a	and   e :: b

then we know

	\x -> e :: a -> b

Conventionally in Haskell we omit brackets in functoin signatures. For instance instead of

```haskell
a -> (b -> (c -> (d -> e)))
```

we simply write 

```haskell
a -> b -> c -> d -> e
```

Note that this is not the same:

```haskell
(((a -> b) -> c) -> d) -> e
```

Here the brackets cannot be removed , as the first part is a single function.

---


	
