# Functional Programming 2018/19

## Lecture 1

Functional Programming is about programming with values rather than statements. It encourages code that is:
	* concise
	* precise
	* reasonable

Here is an example of code in Haskell:

```haskell
> fib :: Integer -> Integer
> fib 0 = 1
> fib 1 = 1
> fib n = fib(n-1) + fib (n-2)
```

To understand this code, we evaluate it:

	fib 2
= 	{def fib}
	fib (2-1) + fib (2-2)
=	{def '-'}
	fib 1 + fib 0
=	{def fib}
	1 + 1
=	{def '+'}
	2

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

One thing we 
