# Introduction to Computer Architecture - Revision notes

## Week 1 - Boolean Algebra
* Boolean Algebra is central to computer architecture; underpins logic

### Propositional Logic
* A **proposition** is basically a statement e.g.
	* the temperature is 20<sup>o</sup>C
	* the temperature is x<sup>o</sup>C
* A proposition must be: 
	* unambiguous - e.g. not something like "the temperature is too hot"
	* Evaluate to a true or false value - e.g. not "this statement is false"
* A proposition *can* include free variables (predicate) and *can* be represented using a short-hand variable or function. In this case, free variables must be bound to arguments before evaluation
* Single statements can be combined with **connectives** like:
	* Not,
	* And,
	* Or,
	* Xor,
	* Implies,
	* Is equivalent to
* Use a truth table to evaluate possible outcomes of a compound proposition

* Axioms are the rules of Boolean algebra
* There are many axioms denoting how expressinsare equivalent e.g.
	* Identity
	* Absorption
	* Distribution

* Use axioms to simplify expressions intelligently
	* acknowledge repeating expressions
* many different ways to compare boolean functions
	* simplification/ mutation via axioms
	* brute force enumeration (truth table)

* Other conjunctions are universal/ functionally complete
	* NAND
	* NOR
* These can be used to construct all other operators
* NAND:
	* not x = x n x
	* x and y = (x n y) n (x n y)
	* x or y = (x n x) n (y n y)

