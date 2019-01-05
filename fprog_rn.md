# Functional Programming 2018/19

## Lecture 1

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

## Lecture 2

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

Together with Bool and Day, we hav introduced Either Bool Day which is a type with 9 values. They ar ethe following:

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

A function takes input values to output values. Every functioon in Haskell takes exactly one input type to one output type.

The type of 'square' is 'Int -> Int'. The type of 'square 5' is just 'Int'.

Here is another function:

```haskell
two :: Int -> Int
two x = 2
```

This returns 2 no matter the input. e.g. two 5 = 2, two |_ = 2

---




