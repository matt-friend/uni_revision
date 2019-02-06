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

---

## Week 2 - Integer representation and arithmetic
* Decimal literals are just a sequence of digits
* The same is true with other data items:
	* a bit is a single binary digit
	* a byte is an 8-element sequence of bits
	* a word is a w-element of bits
* Key concept
	* X^ |--> X
	* Representation of X maps to the value of X

### Aside - properties of bit-sequences
* bit literal can be interpreted two ways
	* big-endian, last bit is most significant
	* little-endian, last bit is least significant
* Hamming weight is the sum of bits which are 1 in a bit-sequence
* Hamming distance is the sum of positions where two m-bit sequences are different

### Positional number systems
* already use positional number systems
* where x^ (an n length digit sequence) maps to +/- the sum of x<sub>i</sub>.b<sub>i</sub>
* where each x<sub>i</sub>
	* is one of *n* digits taken from the set X = {0,1,...,b-1}
	* is weighted by some power of the base b

* One Hex digit (base 16) can represent 4 bits

* Left-shift of an *x* by *y* digits is equivalent to multiplication by *b*<sup>y</sup> 

### **Z** (integers)
* Problem - want to represent **Z** BUT:
	* infinite set
	* so far ignored issue of sign
* Solution: In C, we get 
	* unsigned char = 0 -> 2<sup>8</sup>-1
	* unsigned int = 0 -> 2<sup>32</sup>-1
	* char = -2<sup>7</sup> -> 2<sup>7</sup>-1
	* int = -2<sup>31</sup> -> 2<sup>31</sup>-1

* Unsigned Int can be represented with natural binary expression
	* Sum of x<sub>i</sub> . 2<sup>i</sup>
* Signed can use either sign-magnitude or twos complement:
	* **sign-magnitude** - most significant bit is representative of sign 
	* so -1<sup>x<sub>n-1</sub></sup> . sum of remaining x<sub>i</sub>.2<sup>i</sup>
	* note two represntations of 0 -> +0, -0
	* **two's complement** - most significant bit is weighted as x<sub>n-1</sub>.-2<sup>n-1</sup>
	* the rest of the digits are summed as normal positive values

### Bit arithmetic
* Look at adder designs
* Adder truth tables for half and full adders.

---

### Physics of transistors
* atom buuilt from nucleus and shells of electrons
* when electrons excited with enough energy, can move between shells or to another atom
* electrical current is flow of electrons
* silicon used because abundant and cheap, inert, and can be doped
	* doped with boron/ aluminium for more holes
	* doped with phosphorus/ arsenic for more electrons
* results in semiconductor
	* P-type has extra holes
	* N-type has extra electrons
	* If sandwiched together, electrons can **only** move from N -> P

### Switches and transistors
* FET transistors allow charge to flow between drain and source, channel between is controlled by gate.
* Channel width (so conductivity) controlled by P.D applied to gate
* In a MOSFET transistor, the channel is *induced* 

#### N-MOSFET
* N-type MOSFET 
* N-type terminals, P-type body
* Applying P.D to gate **widens** channel, source + drain connected
* Removing P.D to gate narrows channel, source + drain not connected
```
	d
	|
     |---
g --||
     |---
	|
	s
```
* g = gate
* d = drain
* s = source

#### P-MOSFET
* P-type MOSFET
* P-type terminals, N-type body
* When P.D applied to gate, **narrows** channel, source and drain disconnected
* Removing P.D widens channel, connecting source and drain
```
	s
	|
     |---
g -o||
     |---
	|
	d
```

#### Switches and Transistors
* Typically P-MOSFET and N-MOSFET transistors are not used in isolation
* A CMOS cell combines one N and one P-MOSFET transistor; they work in a complementary way.

#### Manufacture
* wafer
* substrate silicon
* photoresist
* expose photoresist to a mask, hardening exposed photoresist
* wash away unhardened photoresist
* etch away exposed silicon
* strip away hardened photoresist

#### Physical limitation
* There are two types of delay in combinatorial logic
	* wire delay - delay of propogation of electrical current through a wire
	* gate delay - the delay when transistors in gates switch between connected and unconnected states.
* Gate delay typically > wire delay
* **Critical path** - the longest sequential sequence of delays between inputs and outputs.
* For certain combinatorial setups, the output will not match the input due to propogation delays.

---

### Counter Machines
* very simple machines (usually hypothetical, used for proving computational theory)
* Register machines with certain properties:
	* register length is unbounded
	* number of registers goes from 1 -> infinity
* Very limited instruction set (<10)

* unbounded registers = unlimited memory storage (provided use Godel encoding)
* not practical in normal situations, good for theory
* physical implementations with limited registers of finite length exist and are easy to implement

#### Godel numbering
* product of first n primes raised to their corresponding values in the sequence
* eg if r = 1440, {5,2,1} = 2<sup>5</sup>\*3<sup>2</sup>\*5<sup>1</sup>
* Key is to encode and fully decode without losing any information.

#### Counter machine instruction set
* Counter machines can perform any operation
* Most common instructions are :
	* INC r (increment register r)
	* DEC r (decrement register r)
	* JZ r (jump if r = 0)

#### Counter machines memory
* Instructions separated from memory
* Therefore use the Harvard memory paradigm
* Implicit PC defined
* PC only updatable via jumps
* Data values only stored in the defined registers

#### All other operations are synthasizeable
* All operations can be created using INC, DEC, JZ
* e.g. COPY or ADD r<sub>i</sub>, r<sub>j</sub>
* Also CLR, J, MUL

#### Counter machines
* A counter machine with at least two unbounded registers is Turing powerful

#### Simulation efficiency
* Although everything possible with Turing powerful machine, not easy or efficient
* Simulation efficiency usually terrible
* Inclusion of more complex instructions can reduce computation times significantly
* Many alternative register machine implementations are more efficient tahn a Turing machine

---

## Memory Hierarchy

### The ideal computer

Instruction supply:
	* Zero-cycle latency
	* Infinite capacity
	* Zero cost
	* Perfect control flow

Data supply:
	* Zer-cycle latency
	* Infinite capacity
	* Zero cost
	* Inifinte bandwidth

Instruction Execution:
	* Zero latency compute
	* Enough functional units
	* Zero cost
	* Perfect data flow

### Ideal memory

Ideal memory has:
	* Zero access time
	* Zero cost
	* Infinite capacity
	* Infinite bandwidth

Unfortunately these requirements are antagonistic:
	* Bigger is slower; larger memory means longer search times to determine location
	* Faster is more expensive; higher quality memory is harder to manufacture
	* Higher bandwidth is more expensive (more banks, more ports, higher frequency etc)

### The problem

Bigger is slower:
	* SRAM, 512 bytes, sub-nanosec
	* SRAM, KByte~MByte, nanosec
	* DRAM, Gigabyte, ~50nanosec
	* Hard Disk, Terabyte, ~10 millisec

Faster is more expensive:
	* SRAM < 10$ p Megabyte
	* DRAM, < 1$ p Megabyte
	* HDD, < 1$ p Gigabyte

### DRAM
* One bit of charge stored on a capacitor
* Consists of transistor for reading/ writing and capacitor
* Capacitor loses charge over time
* Loses charge when accessed
* Needs to be refreshed

### SRAM
* Two cross-coupled inverters
* So 6 transistors needed
* Retains memory if powered

### Memory Bank (generic)

Read access steps:
	* Decode row (MSB), activate word lines
	* Decode column (LSB), select subset of row via bit lines
	* Amplify row data
	* Send to output

### SRAM vs DRAM
* SRAM is faster access (no capacitor vs DRAM)
* SRAM is lower density
* SRAM is more expensive
* SRAM has no need for refresh, DRAM leaks 
* SRAM is good for logic - DRAM capacitors bad for logic

### HDD vs Flash
* Hard disks are mechanical so slower and easier to wear (moving parts) vs flash which is solid state
* HDDs are cheaper 

### Locality in Space and Time
Locality - things close to each other are likely to be similar

E.g. 
* space locality; geography, sit in friend groups (similar people)
* time locality; weather, attendamce to lectures

### Memory locality
* Temporal - program likely to access the same memory repeatedly in a short period of time
* Spatial - programs reference cluster of nearby adddresses; instructions tend to be in sequence; data structures stored in sequential memory/ nearby locations

### Caching - exploiting locality
* Temporal - recently accessed data is stored in cache memory; typical access time reduced to that of cache memory rather than primary memory
* Spatial - memory is loaded as chunks/ blocks of memory that are close together; memory that has not been used but is loaded but is likely to be used in the near future; "hit rate" for modern cache is ~95%"

---


