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
 
