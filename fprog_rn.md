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

Conventionally in Haskell we omit brackets in function signatures. For instance instead of

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

##### lecture 5

### Lists

A list in Haskell is an ordered sequence of elements that have the same type. For instance:

```haskell
[3, 1, 4, 1, 5, 9, 2] :: [Int]
```

Note that every element has the same type, in this case Int

The followin gis not a list:

```haskell
[3, 'a', "hello", 4.2]
```

Each of the items have a different type. This is not allowed

Lists in Haskell are calles 'homogenous' because the elements are all the same type.

Every list is mad up of two things:

```haskell
[] <- an empty list of type [a]

x : xs 

x - this is a value of type a
: - 'cons'
xs - this is a list of type [a]
```

For example, the following holds true:

```haskell
[3, 1, 4]	= 3 : [1, 4]
		= 3 : 1 : [4]
		= 3 : 1 : 4 : []
```

We can define functions over lists in the usual way, by using pattern matching.

When defining a functon over lists, it is often correct to start by considering the definition for the empty list first followed by the case for cons (:) :

For example, the following function checks if a list is empty:

```haskell
isEmpty :: [a] -> Bool
isEmpty [] = True
isEmpty (x:xs) = False
```

The following is not correct for the nonempty case:

```haskell
isEmpty x : xs = False
```

if the 'x', ':' and 'xs' are separated by spaces they will be treated as separate parameters. Haskell will thinnk we are creating a function called isEmpty with **3** parameters. We use brackets to group the, into a single argument.

Note that (:) is cons, x is a new variable we named of type a and xs is a new variable we named of type [a]. For example, consider the evaluation of this:

```haskell
	isEmpty [3, 1, 4]
=	
	isEmpty (3:[1, 4]) 	3	<- x
=	{def isEmpty}		[1, 4] 	<- xs
	False
```

Now consider the following function:

```haskell
isSingle :: [a] -> Bool
isSingle [] = False
isSingle [x] = True	*
isSingle (x:xs) = False
```
An alternative definition would have the following instead of (\*)

```haskell
isSingle (x:[]) = True
```

These are equivalent because [x] == (x:[])

Another alternative definition would be the following:

```haskell
isSingle :: [a] -> Bool
isSingle [x] = True
isSingle _ = False
```

This identifies the single case first and everything else is false.

The function 'head' looks like this:

```haskell
head :: [a] -> a
head [] = undefined
head (x:xs) = x
```

This is a partial functoin because it is undefined for one of its cases. We try to avoid these functions becuase they cause bugs in our code.

```haskell
tail :: [a] -> a
tail [] = undefined
tail (x:xs) = xs
```

The length of a list is a recursive function:

```haskell
length :: [a] -> Int
length [] = 0
length (x:xs) = 1 + length xs
```

To understand this it is helpful to evaluate an example:

```haskell
	length [3, 9, 2]
=	
	length (3 : 9 : 2 : [])
=	{def length}
	1 + length (9 : 2 : [])
=	{def length}
	1 + (1 + length (2 : []))
=	{def length}
	1 + (1 + (1 + length []))
=	{def length}
	1 + (1 + (1 + 0))
=	{def '+'}
	3
```

---

##### lecture 6

learning how to write programs involving IO ()

---

##### lecture 7

The take function allows you to take a number of elements from a list.

e.g.

```haskell
	take 3 [3, 1, 4, 5, 6]
= 	[3, 1, 4]
```

```haskell
take :: Int -> [a] -> [a]
take n [] = []
take n (x:xs) = x : take (n-1) xs
```

let's try this code:

```haskell
	take 3 [1, 2, 3, 4, 5]
=	{def take}
	1 : take 2 [2, 3, 4, 5]
= 	{def take}
	1 : 2 : take 1 [3, 4, 5]
= 	{def take}
	1 : 2 : 3 : take 0 [4, 5]
= 	{def take}
	1 : 2 : 3 : 4 : take -1 [5]
= 	{def take}
	1 : 2 : 3 : 4 : 5 : take -2 []
= 	{def take}
	1 : 2 : 3 : 4 : 5 : []
=	[1, 2, 3, 4, 5]
```

This failes because we needed to return the empty list when x = 0. We fix this by adding a case for x = 0. Adding it at the end does not work; as it is a specific case of x it needs to be checked out first. The answer is therefore:

```haskell
take :: Int -> [a] -> [a]
take 0 xs = []
take n [] = []
take n (x:xs) = x : take (x-1) xs
```

One thing to notice is that "xs" is not used in the first clause. We could instead write an underscore:

```haskell
take 0 _ = []   <- _ parameter needs no name
```

Also note that "xs" was used in two different clauses: that is not a problem because clauses are selected by pattern matching, and then variables are assigned independently.

A related function is:

```haskell
drop :: Int -> [a] -> [a]
```

such that 
	
```haskell 
	drop 2 [1, 2, 3, 4, 5]
 =	[3, 4, 5]
```

"take" selects n objects from the start of the list; "drop" **removes** those same n objects from the list.

It is defined by:

```haskell
drop :: Int -> [a] -> [a]
drop 0 xs = xs
drop n [] = []
drop n (x:xs) = drop (n-1) xs
```

The filter functon will show us hwo to write a *pattern guard*. This allows us to do further checks when pattern matching.

The filter function removes elements from a list that do not satisfy a certain predicate.

Suppose we have

```haskell
even :: Int -> Bool
```

which returns True if given an even number.

We might want to filter a list so that all the elements are even. e.g 

```haskell
filter even [3, 1, 4, 6, 7]

filter :: (a -> Bool) -> [a] -> [a]
filter f [] = []
filter f (x:xs)
	| f x = x : filter f xs
	| otherwise = filter f xs
```

Pattern guards match from top to bottom and reduce to the first case that evalates to True.

---

##### lecture 8

### Maps over lists

Mapping a function over a list transforms each element in the list by that function.

Example:

```haskell
	[1, 2, 3, 4, 5]

	--> map square
	
	[1, 4, 9, 16, 25]
```

The square function works for an individual number, so it cannot work on a list. So we use 'map' to make a function that can:

```haskell
map :: (a -> b) -> [a] -> [b]
map f [] = []
map f (x:xs) = f x : map f xs
```

Example: 

Remember the function 

```haskell
plus :: Int -> Int -> Int
```

We might want to add 4 to every number in a list:

```haskell
	map (plus 4) [3, 4, 5]
=
	map (plus 4) (3 : 4 : 5 : [])
= 	{def map}
	(plus 4 3) : map (plus 4) (4 : 5 : [])
= 	{def plus}
	7 : map (plus 4) (4 : 5 : [])
= 	{def map}
	7 : (plus 4 4) : map (plus 4) (5 : [])
= 	{def plus}
	7 : 8 : map (plus 4) (5 : [])
= 	{def map}
	7 : 8 : (plus 4 5) : map (plus 4) []
= 	{def plus}
	7 : 8 : 9 : map (plus 4) []
= 	{def map}
	7 : 8 : 9 : []
= 	[7, 8, 9]
```

### List comprehensions

A list comprehension is convenient notation for expressing lists.

The numbers in a sequence are written as:

```haskell
[1..100]
```

This gives us all the values from 1 to 100.

We can also have infinte lists:

```haskell
[1..]
```

This gives us all the Ints forever.

We can define the natural numbers by;

```haskell
nats :: [Int]
nats = [0..]
```

If we wanted a list of all squares, we could use a map:

```
squares :: [Int]
squares = map square nats
```

With list comprehensions we can write:

```haskell
squares = [square x | x <- nats]
```

This is read: sqaures, a list of square x where x is from the list nats

A double comprehension list, taking elements from two lists:

```haskell
	[(x, y) |  x <- xs, y <- ys]
```

Comprehensions can also be filtered by a predicate. Here is the list of all odd numbers, given 'odd :: Int -> bool':

```haskell
odds :: [Int]
odds = [x | x <- nats, odd x]
```

---

##### lecture 9

### Folds

Recursion is one of the most dangerous constructs in a programming language - programmers can use it to write programs that never terminate while doing no useful work. A fold is a way of ensuring termination while producing useful results.

POWERFUL + DANGEROUS

GOTO

WHILE

FOLD, FOR

SAFE + PREDICTABLE

We can fold a list to a final value by using the foldr function.

```haskell
	foldr (+) 0 [1, 2, 3, 4, 5]

=	1 : 2 : 3 : 4 : 5 : []

=	1 + 2 + 3 + 4 + 5 + 0
```

We define foldr as follows:

```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
foldr f k [] = k
foldr f k (x:xs) = f x (foldr f k xs)

where 	f :: a -> b -> b
	k :: b
	(x:xs) :: [a]
```

Let's define the function that sums values:

```haskell
sum :: [Int] -> Int
sum = foldr f k where
	f :: Int -> Int -> Int
	f x y = x + y

	k :: Int
	k = 0
```

This is the long version of this code:

```haskell
sum :: [Int] -> Int
sum = foldr (+) 0
```

The pattern is similar for products:

```haskell
product :: [Int] -> Int
product = foldr (*) 1
```

Now we try a definition for length:

```haskell
length :: [a] -> Int
length [] = 0
length (x:xs) = 1 + length xs
```

That used pattern matching and recursion. We can try to figure it out with foldr:

```haskell
length [] = 0	<-- this is a hint that k will be 0

length (True : []) = 1
	     ^--- we know f replaces : so this implies that f True 0 = 1

length (False : True : []) = 2
	      ^---- Same thing; f False 1 = 2
```

From this we can assume f _ x = x + 1. 

So we can code length like this:

```haskell
length :: [a] -> Int
length = foldr f k where
	f :: a -> Int -> Int
	f _ x = x + 1

	k :: Int
	k = 0
```

We can test this by evaluating:

---

##### lecture 10

We will evaluate the result of sum [3, 1, 4]

```haskell
	sum [3, 1, 4]
= 	
	sum (3 : 1 : 4 : [])
= 	{def sum}
	foldr (+) 0 (3 : 1 : 4 : [])
= 	{def foldr}
	3 + (foldr (+) 0 (1 : 4 : [])
= 	{def foldr}
	3 + (1 + (foldr (+) 0 (4 : [])
=	{def foldr}
	3 + (1 + (4 + (foldr (+) 0 [])
= 	{def foldr}
	3 + (1 + (4 + 0)
=	{def '+'}
	8
```

### Newtypes, types and data

There are three ways of introducing new types of data in Haskell

#### Data

Here we write:

```haskell	
data Maybe a = Just a | Nothing
```

This is shorthand for the following:

```haskell
data Maybe a where
	Nothing :: Maybe a
	Just a :: a -> Maybe a
```

This introduces the type 'Maybe a' for any 'a' as well as th constructors 'Just' and 'Nothing'. All constructors start with a capital letter, result in the type being defined and be pattern match upon.

Another example:

```haskell
data Either a b = Left a | Right b
```

This can be translated as:

```haskell
data Either a b where
	Left a :: a -> Either a b
	Right b :: b -> Either a b
```

These are constructors, not functions, so there is no function body required

#### Type Synonyms

A type synonym introduces a name for an existing type.

Instead of writing '[Maybe a]' we might want to write 'Maybes a' instead. To do this, we write:

```haskell
type Maybes a = [Maybe a]
```

This allows ypu to write 'Maybes a' instead of [Maybe a].

#### Newtypes

Units of measure sometimes share an underlying data representation, but should nevertheless be kept distinct. 

For instance consider the number '5', depending on context, it may represent different things such as 5cm, 5in, 5kg etc.

To introduce metres we do this:

```haskell
newtype Metre = Metre' Int
```

To understand this it may help to look at the longhand version:

```haskell
newtype Metre where
	Metre' :: Int -> Metre   <-- this is not valid haskell
```

This introduces a new constructor called Metre', which takes an Int and makes a value of type Metre.

---

##### lecture 11

### Type classes

A type class provides a way to give functinos multiple meanings dependent on the type of the function.

For example consider the function 'print' that prints values.

```haskell
	show True = "True"
	show 5 = "5"
	show (Just 3) = "Just 3"
	show "hello" = "hello"
```

We might imagine that print has the following type:

```haskell
	print :: a -> string
```

This is not correct because there are some things that do not work for print, such as a function.

We introduce the family of print functions by creating a new type class:

```haskell
class Show a where
	show :: a -> String
```

This introduces the function 'print' with type:

```haskell
print :: Show a => a -> String
```

NB. Show a is not a parameter; => is not a function

The above says that if 'a' is a member of the 'Show' type class then print has type a -> String.

The class definition gives the types of its functions, but not their implementation. For that, we use an instance for each member of the family:

If we want to be able to print Bool values then we write the following:

```haskell
insatnce Show Bool where
	--show :: Bool -> String
	show True = "True"
	show False = "False"
```

This introduces 'print' that works for Bool values. In principle we must define an instance for every type of interest.

Haskell has 'Show' in-built, and can derive definitions automatically for some datatypes if you ask it to.

To do this we write:

```haskell
data Day = Mon |Tue |Wed |Thur |Fri |Sat |Sun
	deriving Show
```

This tells Haskell to make the appropriate instance of Show for us.

### Equality

Values can often be checked for equality. Not all things can be checked for equality.

For this we use the symbol'=='.

```haskell
	5 == 3 	= False
	5 == 5	= True
```

However, 5 == True is not well-typed; comparing a number to a boolean is nonsense.

To introduce ==, we use a type class:

```haskell
class Eq a where
	(==) :: a -> a -> Bool
```

As before this introduces a function:

```haskell
(==) :: Eq a => a -> a -> Bool
```

Just as with Show, this can be derived. To derive both, use this line:

```haskell
...
	deriving (Show, Eq)
```

### Monoids

A monoid is an operation '**o**' together with a neutral element '**e**' such that the following laws hold true:

* associativity: (x **o** y) **o** z = x **o** (y **o** z)
* left unit: **e** **o** y = y
* right unit: y **o** **e** = y

Formally, a monoid is a triple \<x, **o**, **e**\> where **o**: x -> x -> x and **e**: x satisfying these laws.

Examples:

\<**N**, +, 0\> - addition
\<**N**, \*, 1\> - multiplication

---

##### lecture 12

A monoid consists of 3 things:

* a set **m**
* an operation **o** : m -> m -> m
* a member of set **m**, **e**: m

Here is a table of a few monoids:

**m** | **o** | **e** | name
--- | --- | --- | ---
**N** | + | 0 | addition
**N** | x | 1 | multiplication
Bool | v | False | or
Bool | ^ | True | and
Matrix | X | id | matrix mult.
[a] | ++ | [] | lists

Since there are so many monoids we can capture this common pattern in a type class.

```haskell
class Monoid n where
	mempty :: m			-- neutral element
	mappend :: m -> m -> m		-- operation
```

We consider an instance of this class to be valid when the three monoid laws hold true.:

1. mappend x (mappend y z) = mappend (mappend x y) z
2. mappend mempty x = x
3. mappend y mempty = y

For example, we can write an instance where m = Int for addition:

```haskell
instance Monoid Int where
	--mempty :: Int
	mempty = 0

	--mappend = Int -> Int -> Int
	mappend = (+)
```

The problem with this is that now we have locked ourselves in to making (+) the monoidal operation for Int, but (\*) would also have been valid.

We can resolve this by using a newtype for each conflicting monoid.

To do sum properly:

```haskell
newtype Sum = Sum Int

instance Monoid Sum where
	--mempty :: Sum
	mempty = Sum 0
	--mappend :: Sum -> Sum -> Sum
	mappend (Sum x) (Sum y) = Sum (x+y)

where x and y are Ints
```

Suppose we have a list of monoidal values, and we want to collapse it:

```haskell
	[Sum 3, Sum 2, Sum 1] :: [Sum]
		|
		v
	Sum 3 o Sum 2 o Sum 1 o e
```

To achieve this, we could use a foldr:

```haskell
fold :: Monoid m => [m] -> m
fold = foldr mappend mempty
```

---

##### lecture 13

### Data Structures

One of the most basic structures is the Maybe data type. It was defined as follows:

```haskell
data Maybe a = Just a | Nothing
```

This introduces the constructors:

```haskell
	Nothing :: Maybe a
	Just :: a -> Maybe a
```

One thing we can do is transform the contents of a 'Maybe a' so that values of type 'a' become type 'b' instead. This would make a value of type 'Maybe b'

```haskell
mapMaybe :: (a -> b) -> (Maybe a) -> (Maybe b)
mapMaybe f Nothing = Nothing
mapMaybe f (Just x) = Just (f x)
```
#### Trees

A slightly more complex structure is a 'Tree'. A tree i smade up of Node and Leaf values:

```haskell
Fork	-> 	Fork	->	Leaf	->	a
			->	Fork	->	Leaf	->	a
					->	Leaf	->	a
	->	Leaf	-> 	a
```

Notice only a leaf can point to a value of type 'a'

We can define a Tree in Haskell as follows:

```haskell
data Tree a = Leaf a | Fork (Tree a) (Tree a)
```

This creates two constructors that we can pattern match onto in the usual way.

```haskell
Leaf :: a -> Tree a
Fork ::(Tree a) -> (Tree a) -> (Tree a)
```

Here are some values that construct trees:

```haskell
Leaf True :: Tree Bool
Leaf 4 :: Tree Int
Fork (Leaf True) (Leaf False) :: Tree Bool
```

However, this is not allowed:

```haskell
Fork (Leaf True) (Leaf 5) :: Not allowes!
```

We cannot have a Tree like this because the subtrees have different types - they must always be the same.

We can have more interesting Tree types; for instance consider:

```haskell
Tree (Maybe Int)
```

For instance we can have:

```haskell
Leaf Nothing :: Tree (Maybe a)
Leaf (Just 3) :: Tree (Maybe Int)
Fork (Leaf Nothing) (Leaf (Just 7)) :: Tree (Maybe Int)
```

We can convert between trees in the same way as lists or maybes:

```haskell
mapTree :: (a ->b) -> Tree a -> Tree b
mapTree f (Leaf x) = Leaf (f x)
mapTree f (Fork l r) = Fork (mapTree f l) (mapTree f r)
```

A similar structure is a Bush: The idea here is that values are all in the nodes, and there are no leaves.

```haskell
data Bush a = Node (Bush a) a (Bush a) | Tip
```

Here are some values that use this structure:

```haskell
Tip :: Bush a
Node Tip 6 Tip :: Bush Int
```

We have created the following constructors

```haskell
Tip :: Bush a
Node :: (Bush a) -> a -> (Bush a) -> (Bush a)
```

Just as before, we can map over the contents of a Bush:

```haskell
mapBush :: (a -> b) -> (Bush a) -> (Bush b)
mapBush f Tip = Tip
mapBush f (Node l x r) = Node (mapBush f l) (f x) (mapBush f r)
```

---

##### lecture 14

### Functors

The different maps are all very similar:

```haskell
map :: (a -> b) -> [a] -> [b]
mapMaybe :: (a -> b) -> (Maybe a) -> (Maybe b)
mapTree :: (a -> b) -> (Tree a) -> (Tree b)
mapBush :: (a -> b) -> (Bush a) -> (Bush b)
```

The generalisation of data structures like [], Maybe, Tree and Bush is called a Functor.

We use a type class to generalise:

```haskell
class Functor f where
	fmap :: (a -> b) -> (f a) -> (f b)
```

Note that the f here is not a function, it is a variable that represents a type. For example f can be [], Maybe, Tree etc.

A valid instance of the Functor type class must obey the functor laws:

1. fmap id = id <- **identity law**
2. fmap g . fmap f = fmap (g.f) - **composition law**

These laws make use of the function fmap that we are defining, as well as the functions id and (.).

They are already defined as :

```haskell
id :: a -> a
id x = x
```

This function does nothing to its input.

```haskell
(.) :: (b -> c) -> (a -> b) -> (a -> c)
(g.f) x = g (f x)
```

### Structural Induction

We prove statements by using induction. This means we first prove the statement for the empty base case. Then we prove it for the recursive case, assuming that it is true for the smaller cases.

For lists, we have:

```haskell
instance Functor [] where
	--fmap :: (a -> b) -> (f a) -> (f b)
	fmap = map
```

The longer version defines fmap from scratch:

```haskell
instance Functor [] where 
	--fmap :: (a -> b) -> [a] -> [b]
	fmap f [] = []
	fmap f (x:xs) = f x : fmap f xs
```

We want to prove that the functor laws hold true for this instance.

First, we show that 'fmap id = id'.

We prove this by induction on lists. First the base case, which is the empty list.

```haskell
	fmap id [] = id []
```

Proof:

```haskell
	fmap id []	|	id []
=	{def fmap}	|=	{def id}
	[] 		|	[]
```

Now the inductive case:

Assume that fmap id xs = id xs

fmap id (x :xs) = id (x:xs)

Proof:

```haskell
	fmap id (x:xs)
=	{def fmap}
	id x : fmap id xs
=	{def id}
	x : fmap id xs
=	{def induction hypothesis}
	x : id xs
=	{def id}
	x : xs
= 	{def id}
	id (x:xs)
```

We do the same with composition.

Base case: []

(fmap g . fmap f)[] = fmap (g.f) []

Proof:

```haskell
	(fmap g . fmap f)[]	|	fmap (g.f) []
=	{def '.'}		|=	{def fmap}
	fmap g (fmap f [])	|	[]
=	{def fmap}		|
	fmap g []		|
=	{def fmap}		|
	[]			|
```

Inductive case: Assume (fmap g . fmap f) xs = fmap (g.f) xs

We must prove that (fmap g . fmap f) (x:xs) = fmap (g.f) (x:xs)

```haskell
	(fmap g . fmap f) (x:xs)
=	{def '.'}
	fmap g (fmap f (x:xs))
=	{def fmap}
	fmap g (f x : fmap f xs)
=	{def fmap}
	g (f x) : fmap g (fmap f xs)
```

---

##### lecture 15

---

##### lecture 16

### Indexed Trees

It is possible to extract indexed elements from a list. This is written (!!) which we call "bang bang" or "shriek shriek".

```haskell
(!!) :: [a] -> Int -> a
(x:xs) !! 0 = x
(x:xs) !! n = xs !! (n-1)
_ !! _ = error "Foolish human"
```

Intuitively, this function takes about n steps to find an element, where n is the length of the list.

We will try to resolve this by defining a tree that stores information about how many elements it contains.

```haskell
4	->	2	->	'a'
			->	'b'
	->	2	->	'c'
			->	'd'
```

To define this structure, we want something that contains leaves of arbitrary type but whose node ar eintegers that carry the nnumber of elements in subtrees.

```haskell
data ITree a = ILeaf a | INode Int (ITree a) (ITree a)
```

To get an intuition for how this corresponds to lists, we will define a function that flattens a tree.

For instance, flattening the tree above produces the list ['a','b','c','d'].

```haskell
flatten :: ITree a -> [a]
flatten (ILeaf x) = x
flatten (INode ix l r) = flatten l ++ flatten r

Need to use (++) , not (:), as flatten l may produce >1 item
```

As a side note, we have no way of creating an 'ITree a' that contains no values of type 'a'. In other words, we cannot flatten an empty list. We could easily add a constructor to do this for us.

To make an 'ITree a', we define 'mkITree':

```haskell
mkITree :: [a] -> ITree a
mkITree [] = error "Foolish human"
mkITree [x] = ILeaf x
mkITree xs = INode ix l r
	where 
	ix = length xs
	l = mkITree (take (ix `div` 2) xs)
	r = mkITree (drop (ix `div` 2) xs)
```

We can improve this code to avoid take and drop by using the splitAt function:

```haskell
mkITree xs = INode ix l r
	where
	ix = length xs
	(ys, zs) = splitAt (ix `div` 2) xs
	l = mkITree ys
	r = mkITree zs
```

This uses the splitAt function defined in the Prelude as:

```haskell
splitAt :: [a] -> Int -> ([a],[a])
splitAt xs 0 = ([], xs)
splitAt (x:xs) n = (x : ys, zs)
	where
	(ys, zs) = splitAt xs (n-1)
```

---

##### lecture 17

We are ready to retrieve elements from the tree that we have constructed.

First we start with a specification:

```haskell
retrieve :: ITree a -> Int -> a
retrieve t n = flatten t !! n
```

This is a good specification because we are defining behaviour in terms of known functions on another well-behaved datatype.

For a better implementation, first we pattern match:

```haskell
retrieve :: ITree a -> Int -> a
retrieve (ILeaf x) 0 = x
retrieve (INode ix l r) 0 
	= case l of
		ILeaf x -> x
		INode ixl ll lr -> retrieve ll 0
retrieve (INode ix l r) n
	= case l of
		ILeaf x -> retrieve r (n-1)
		INode ixl ll lr -> 
			if n < ixl then retrieve l n
			else retrieve r (n - ixl)
```

```haskell
7	->	3	->	'g'
			->	2	->	'f'
					->	'e'
	->	4	->	2	->	'd'
					->	'c'
			->	2	->	'b'
					->	'a'
```

Finding element 6 should retrieve 'g'.

When we look at this tree, we see that the left tree has 4 elements in it, and since 4 <= 6 we must search in the right tree with index 6-4 = 2.

The case statements in the code were there because the information we needed was not available immediately in a node. It would have been better to store the information about the length of the tree on the left.

A good exercise is to come up with a better structure that stores the index information that we need directly.

### Folding data structures

The foldr function was useful to allow us to define many functions on lists. It is also possible to define a fold for any algebraic data structure ie sums/ products.

To achieve this we first have to understand how we might have derived the foldr function.

A foldr function is defined as follows:

```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
foldr f k [] = k
foldr f k (x:xs) = f x (foldr f k xs)
```

To generalise:

```haskell
foldr :: (a -> b -> b) -> b -> [a] ->  b

	(a -> b -> b) replaces (:) (called cons)
	b is for the [] (empty list)
foldr cons empty [] = empty
foldr cons empty (x:xs) = cons x (foldr cons empty xs)
```

N.B It is easy to confuse 'cons' and 'empty' with (:) and []. This is not the case. They are parameters that know how to deal with these constructors.

Notice the types too:

```haskell
	[] :: [a]	|	(:) :: a -> [a] -> [a]
	empty :: b`	|	cons :: (a -> b -> b)
```

The pattern here is that any constructor that has '[a]' becomes the corresponding function where this is now 'b'.

So now consider the type for a 'Tree a':

```haskell
data Tree a = Leaf a | Fork (Tree a) (Tree a)
``

This has defined two constructors, 'Leaf' and 'Fork', with the following types:

```haskell
Leaf :: a -> Tree a
Fork :: Tree a -> Tree a -> Tree a

leaf :: a -> b
fork :: b -> b ->. b
```

We decided to create 'leaf' and 'fork' by mirroring and adapting 'Leaf' and 'Fork'. These will be the parameters to our fold:

```haskell
foldTree :: (a -> b) -> (b -> b -> b) -> Tree a -> b
foldTree leaf fork (Leaf x) = leaf x
foldTree leaf fork (Fork l r) = fork (foldTree leaf fork l) (foldTree leaf fork r)
```

---

##### lecture 18

implementing 'ATree a'

---

##### lecture 19

To use the 'foldTree' function, we do much the same as when using 'foldr'.

For example, suppose we have a tree full of numbers that we want to sum.

```haskell
Fork 	->	3
	->	Fork	->	2
			-> 	1
```

This tree is represented by the following value:

Fork (Fork (Leaf 1) (Leaf 2)) (Leaf 3)

To sum the values in this tree, we implement the 'sumTree' function:

```haskell
sumTree :: Tree Int -> Int
sumTree = foldTree leaf fork
	where
	leaf :: Int -> Int
	leaf x = x
	
	fork :: Int -> Int -> Int
	fork l r = l + r
```

Let's look at how this works by executing on a simple example:

```haskell
	sumTree (Fork (Fork (Leaf 1) (Leaf 2)) (Leaf 3))
=	{def sumTree}
	foldTree leaf fork (Fork (Fork (Leaf 1) (Leaf 2)) (Leaf 3))
=	{def foldTree}
	fork (foldTree leaf fork (Fork (Leaf 1) (Leaf 2))) (foldTree leaf fork (Leaf 3))
=	{def foldTree}
	fork (fork (foldTree leaf fork (Leaf 1)) (foldTree leaf fork (Leaf 2))) (foldTree leaf fork (Leaf 3))
=	{def foldTree}
	fork (fork (1) (2)) (foldTree leaf fork (Leaf 3))
=	{def fork}
	fork (1 + 2) (foldTree leaf fork (Leaf 3))
=	{def '+'}
	fork (3) (foldTree leaf fork (Leaf 3))
=	{def foldTree}
	fork (3) (3)
=	{def fork}
	3 + 3
= 	{def '+'}
	6
```

We can demonstrate 'foldTree' by taking the size of a tree, which is the number of values it contains:

```haskell
sizeTree :: Tree a -> Int
sizeTree = foldTree leaf fork
	where
	leaf :: a -> Int
	leaf x = 1

	fork :: Int -> Int -> Int
	fork x y = x + y
```

As a final example, let's look at the height:

```haskell
heightTree :: Tree a -> Int
heightTree = foldTree leaf fork
	where
	leaf :: a -> Int
	leaf x = 1
	
	fork :: Int -> Int -> Int
	fork x y = 1 + x `max` y
```

The key point to note is that by using the 'foldTree' function, we did not have to concern ourselves with all the repetitive structure of the code that does recursion.

### Lookup Maps

A very useful data structure is 'Map k v', which acts like a dictionary or an address book that gives values of type 'v' when keys of type 'k' are supplied. 

In essence we have the following:

```haskell
	Map k v  ~=  k -> v
```

This says that maps and functions are interchangeable.

The constructors to a map remain abstract: they cannot be accesssed directly.

Abstract data types like this are useful for many reasons. For instance, keeping a type abstract allows programmers to offer different underlying implementations at different poiints in time without end-users realising.

We can import the 'Map' datatype by importing the module:

```haskell
import Data Map
```

This imports all the functionality of maps from an external module called 'Data.Map'.

This imports many functions, including 'lookup':

```haskell
	lookup :: Map k v -> k -> Maybe v
```

The problem is that 'lookup' is a different function that is imported by the Prelude by default:

```haskell
	lookup :: [(a,b)] -> a -> Maybe b
```

We need a way to distinguish which one we mean.

---

##### lecture 20

We can import the lookup function from Map explicitly only when it is prefixed with a symbol we choose:

```haskell
import qualified Data.Map as M

qualified -> I want explicit names
Data.map -> the map library
M -> the chosen prefix
```

This qualified import allows us to refer to the 'lookup' function from 'Data.Map' by writing

```haskell
M.lookup
```

The other technique is to hide the version of 'lookup' from the Prelude:

```haskell
import Prelude hiding (lookup)
```

The most basic map is given by:

```haskell
empty :: Map k v
```

This is the map that contains no entries.

We also have a way of inserting elements in a map:

```haskell
insert :: Ord k => k -> v -> Map k v -> Map k v
```

The result of 'insert k v m' is to insert the value v at key k in m.

For example:

```haskell
insert "Joe" 8 empty :: Map String Int
```

This contains a key called "Joe" that maps to the value 8. If "Joe" is already in the map, then the previous value is completely overwritten.

The 'lookup' function fetches values from a map. The result of 'lookup k m' is 'Nothing' if 'k' is not in the map 'm'. Otherwise the result is 'Just v' where 'v' is the value associated to 'k'.

For example

```haskell
lookup k empty = Nothing
lookup k (insert k v m) = Just v
```

### Trie maps

A trie is a kind of tree that incorporates the behaviour of a map, where the keys are in fact a list of elements

For example, here is a 'Trie Char Int':

```haskell
\0 'N'-> \0 'i'-> \0 'c'-> 4 'k'-> 3
		             'o'-> 6 'l'-> 5 'e'-> 3
					     'a'-> 15 's'-> 1
```

\0 represents 'Nothing' whereas 0 represents 'Just 0'.

Char represents the edge type, and Int the node type.

A value of 'Nothin' means there is no entry that corresponds to th key, and a value of 'Just 0' means nobody has that name.

A 'Trie k v is implemented as follows:

```haskell
data Trie k v = Trie (Maybe v) (Map k (Trie k v))

Maybe v = nodes: this corresponds to the values at each node e.g \0 or '0' or '5'
Map k (Trie k v) = edges: this corresponds to the link from the current node to more tries.
```
To understand this, consider this trie:

```haskell
5	'a'-> 	t1
	'v'->	t2
	'q'->	t3
```

This corresponds to:

```haskell
	Trie (Just 5) m
where m is the map
	'a' |--> t1
	'v' |--> t2
	'q' |--> t3
```

---

##### lecture 21

We write a concordance using Trie structures

---

			     	 
	


