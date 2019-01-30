# Theory of Computation

## Lecture 1: introduction

Assessment - 2 in-class quizzes, 1 exam

Theory of Computation - what problems can computers solve?

Real life -> Problem -> Abstraction -> Mathematical model

What problems can computers *efficiently* solve?

Computability = solvable
Efficiently = complexity

### Language of theory

Alphabet - a finite set
* E1 = {0,1}
* E2 = {a,b,c...z}
* E2 = {boy,cat,likes,#}

Strings (words)
* W1 = 1011
* W3 = boy#likes#cat

Length of strings
* |W1| = 4
* |W3| = 5

Empty string
* |E| = 0

Concatenation
* X = a1, a2, a3 ... an
* Y = b1, b2, b3 ... bm
* X.Y = XY = a1, a2, a3, ... an, b1, b2, ... bm
* W1.W1 = 10111011
* X<sup>k</sup> = X.X.X.X {n = k}
* W1<sup>0</sup> = E

Reverse
* W = a1, a2, ... an
* W<sup>R</sup> = an, a(n-1), ... a1
* W3<sup>R</sup> = cat#likes#boy

Language = a set of strings

L1 = { x | x is a string, x ends with 0}
	* 11 not in L1
	* 10 in L1

L2 = { x | x is the binary representation of a prime number}
	* 11 in L2
	* 10 in L2 (and L1)

L3 = { P | P is a valid C program}

L4 = { P | P is a valid C program which sorts the input array}

### Deterministic finite automata
* is a finite state machine

Formally:

DFA is a 5-tuple:
	* Q: a finite set of states
	* E: a finite alphabet
	* d: QxE -> Q (transition)
	* q<sub>0</sub> in Q: start state
	* F a subset of Q: set of accepting states

The DFA moves between a finite number of states based on the current state and the next input

---
