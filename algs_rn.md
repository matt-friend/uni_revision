# Algorithms

## Lecture 1: Introduction - Peak finding

### Algorithms?
* Algorithm is a procedure that solves a computational problem

### What we want and how it works
Efficiency
* Faster = better
* Use as little memory as possible (space complexity)

Mathematics
* We will prove that algorithms run fast and use little memory
* Prove that the algorithms are correct
* Tools; Induction, algebra, sums... rigorous arguments

### Peak Finding
* Given length n array of integers, find peak where none of the integers either side of an integer are greater than it
* Input: array length n
* Output: position 0 <= i <= n-1 such that a<sub>i</sub> is a peak

### Is peak finding well defined?
* Does every array have a peak?
* Lemma: every array has a peak
* Proof: 
	* Assume no peak in array
	* therefore a<sub>1</sub> > a<sub>0</sub>
	* continuing pattern, a<sub>n-1</sub> must be greater than a<sub>n-2</sub> and so is a peak (contradiction)
	* therefore every array has a peak
* Shorter proof:
	* Every maximum is a peak (shorter, immediately convincing)

### How fast is simple algorithm?
* Simple algorithm:
	* look at a0 - if a0 > a1, return 0
	* look at a[n-1] - if a[n-1] > a[n-2], return n-1
	* for i = 1 to n-2, look at adjacent integers, if current is >= these numbers then return i
* Speed (worst case!)
	* looks at a0 and a1, 2
	* looks at a[n-2], a[n-1], 2
	* looks at all elements 1 - n-2 4 times
	* So 2 + 2 + 4(n-2) = 4(n-1)
* Recursive peak finding algorithm faster, 5log(n) (a lot better)

---

## Lecture 2: O-Notation (Why constants matter less)

### Runtime of an ALgorithm
Runtime
	* Function that maps the input length n to the number of simple/ unit/ elementary operations
	* The number of array accesses in Peak Finding represents the number of unit operations very well

Log(n) runtimes are favourable

### Order Functions Disregarding Constants
Asymptotic complexity
	* For large enough n, constants matter less
	* For small n, most algs are fast anyway
An increasing function f:N->N grows asymptotically at least as fast as an increasing function g:N->N if there exists an n<sub>0</sub> in N such that for every n >= n<sub>0</sub> it holds: f(n) >= g(n)

e.g. f(n) = 2n<sup>3</sup> , g(n) = 0.5x2<sup>n</sup>

Then g(n) grows asymptotically at least as fast as f(n) since for every n >= 16 we have g(n) >= f(n)	

### The racetrack principle
Let f,g  be functions , k an integer and suppose that following holds:
	* f(k) >= g(k) and
	* f'(n) >= g'(n) for every n >= k
Then for every n >= k, it holds that f(n) >= g(n)

Example: n >= 3log(n) + 2 holds for every n >= 16
	* n >= 3log(n) + 2 holds for n = 16
	* We have: (n)' = 1 and (3log(n) + 2)' = 3/nln2 < 1/2 for every n >= 16. The result follows

### Ordering functions by asymptotic Growth
If <= means grows asymtotically at least as fast as:

5logn <= 4(n-1) <= nlog(n/2) <= 0.1n<sup>2</sup> <= 0.001x2<sup>n</sup>

So: polynomials of logs <= polynomial <= exponential

### Big-O notation
Let function g : N->N

O(g(n)) is the set of functions {f(n) : There exist positive constants c and n<sub>0</sub> such that 0 <= f(n) ,+ cg(n) for all n >= n<sub>0</sub>}

Meaning: f(n) is in set O(g(n)) such that g grows asymptotically at least as fast as f up to constants

Example: f(n) = 1/2 n<sup>2</sup> - 10n and g(n) = 2n<sup>2</sup>

Then g(n) is in the set O(f(n)) since 6f(n) >= g(n) for every n >= n<sub>0</sub> = 60

### Runtime of algorithms
Tools for the analysis of algorithms
	* Runtimes of algorithms expressed using O-notation
	* Allows for comparison of algorithm runtimes
	* Important to find algorithms with slowest growth

Important properties for analysis of algorithms
	* Instruction composition: if f in O(h<sub>1</sub>), g in O(h<sub>2</sub>), then f + g in O(h<sub>1</sub> + h<sub>2</sub>)
	* Loops (repetition of instructions: f.g in O(h<sub>1</sub>.h<sub>2</sub>)

### Rough hierarchy
* Constant time O(1) : individual operations
* Sub-logarithmic time O(log(log(n)))
* Logarithmic time O(log(n))
* Poly-logarithmic time O(log<sup>2</sup>n)
* Linear time O(n) : time to read input
* Quadratic time O(n<sup>2</sup>) : potentially slow on big inputs
* Polynomial time O(n<sup>c</sup>) : used to be considered efficient
* Exponential time O(2<sup>n</sup>) : works on only very small inputs
* Super-exponential time O(2<sup>2<sup>n</sup></sup>) : very bad

---
