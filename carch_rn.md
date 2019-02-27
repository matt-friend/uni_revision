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

---

## Week 3

### 
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

## Instruction set architectures

### Whats an ISA?
* Its a contract/ the interface between the software and the hardware
* It is the definition of the instructions supported by a particular processor
* The ISA specifies the exact behaviour of instructions fed to the processor
* It should be designed sympathetically for the programming language that will write code for it

### The 'I'
* An instruction is a definition of a unique operation code plus some contextual information
* We say there is an operation plus zero or more arguments

### Decoding
* All instructions must be uniquely decodable
* i.e one-to-one mapping of bit string to function
* Some ISAs do not define all possible bit strings which is allowed but can cause problems if an undefined bit string is encountered

### Creating instructions
* So, to create an ISA we need to produce a list of possible instructions and their possible arguments
* Which instructions make it to the ISA depends on the purpose of the ISA

### Instruction classes
Most instructions fall naturally into one of 4 classes
	* Arithmetic (add, sub, multiply)
	* Comparison (compare, if, set flags)
	* Memory (load, store, swap)
	* Control flow (branch, state changes)
* Simple processors may only need the above classes of processors, but some advanced processors may employ higher level instructions:
	* Vector processing (array operations, block processing)
	* Digital signal processing (e.g Fast Fourier Transform)

### Instruction selection
* Choice of instruction should be based on a number of factors:	
	* Common operations
	* Represenatative applications
	* Future-proofing

### RISC and CISC
* ISAs are often categoried by how close they are to one of two paradigms:
	* RISC (reduced instruction set computer)
	* CISC (complex instruction set computer)

### RISC
* Aim is a minimal set of instructions that are fast and easy to execute
* Each instruction should have a similar runtime
* Complex instructions should be synthesised from simpler ones
* Usually instructions have a fixed length
* Run fast but usually need a lot of instructions to get a complex process done

### CISC
* Opposite of risc
* If there is a specialised instruction to be done, add a dedicated instruction for it (up to a point)
* Rich instruction sets mean smaller programs
* Mixed length instructions mean unpredictability and compiler headache
* Processor ends up complex and slow

### Instruction choices
* For many operations, have a set of instructions to choose from
* How should we construct the operation?

### Instruction set choice
* How to choose between different instruction sets?
* Need to find a tradeoff 

### ISA tradeoff
* More instructions = more bits = greater information
* More operands = more bits = more work

### ISA implications
* Choice of instruction specificity can impact how a machine may be programmed
	* Implicit source and destination = stack machine
	* Implicit destination = accumulator machine
	* Explicit source and destinations = register machine

### Picking a set
We want to cover the useful space of execution needs:
	* No repetition
	* No coverage where it is not needed
	* Minimum size to get the job done
	* Check if instruction inclusion is harmful (Amdahls law)

### Amdahls law
* Dont add an instruction to an ISA if its inclusion slows down the system

### Instruction encodings
* Need to encode instructions unambiguosly once they have been selected
* A common way to separate them is into opcodes and operands
* Space is a major factor in deciding instruction encodings
	* memory size
	* number of instruction loadable per second
	* decode stage complexity

### Instruction lengths
* Instructions can and do vary in length
* Some instructions need to provide more information so are longer
* Shorter instructions are better
* This implies that fixed length instructions may be sub-optimal
* Example of variable length instruction: ADD r0, r0, r1 vs HALT

### Variable length instructions 
Pros:
	* Size efficient
	* Can be extended to support more operands
	* Optimal?
Cons:
	* Almost infinte possibilies meaning complex decoding
	* Pipeline slowdown

### Fixed length instructions
* May be sub-optimal
* Limit on length of immediates
* Major advantage is simplified decode stage
* Tends to be the winner in most modern designs

---

## ARM ISA

### Example ISA - Arm thumb V1

### Why thumb v1?
* Small and simple instruction set
* Contemporary, good performance for its size

### The ARM architecture
* Register machine, 16 registers plus status register
* The PC and LR are part of these registers
* RISC
* Good support for multiple memory addressing nodes and is effective with stacks

### Flexible registers
* ARM registers are orthogonal so any combination of them can be used for the source and destination of any instruction
* This gives great flexibility in register assignment and variable manipulation
* It includes the Program Counter - branches can be made a result og an arithmetic calculation

### Memory manipulation
* Most of the addressing modes we saw are supported by the arm architecture, imcluding:
	* Immediate
	* Direct
	* Register-indirect
	* Indexed

In general, memory address are accessed in a two phase approach

1. Store the memory address to be accessed in a register
2. Execute a load or store instruction, targeting that registers address

### Memory access example
To store, for example, 42 in memory location 8, the processor would: 
	* MOV r0, \#8
	* MOV r1, \#42
	* STO r1, r[0]

### ARM PC
* The ARM program counter is pre-incremented in an execution cycle
* It runs ahead of the current instruction by two instructions (because it follows a three stage pipeline model)

### ARM vs Thumb
* Thumb instruction set is a recoded subset of the ARM instruction set 
* Allows for better performance on ARM implementations that use a 16-bit or narrower data bus. Allows better code density than provided by the standard ARM instruction set.

### Thumb V1 overview
* 16-bit instruction set
* This places some limitations on what it can and cannot do
	* encoding three registers from the arm set takes 12 bits
	* there cannot be very many opcodes

### Thumb V1 instructions
* Broadly split into two forms:
	* Three register operations
	* Two register and one immediate operations

### Thumb V1 registers
* r0 to r7: general purpose registers
* r8 to r12: limited access registers
* r13: stack pointer
* r14: link register
* r15: program counter

### Thumb V1 instructions
* Most thumb v1 instructions address one of the eight 'low' registers 
* There are two main forms of register encoding:
	* Three explicit registers
	* Two explicit and one implicit

### 'High' instructions
* Some instructions need access to registers \>r7 and allow access to the full register set
* These instructions encodings are known as 'high' instructions e.g ADDH, MOVH
* Often some other functionality of the instruction is sacrificed, such as 3-register format being reduced to 2-register format

### Conditionality
* Conditionality in ARM is powerful and complex in equal measure
* Conditional execution occurs in the following way:
	* Certain instructions execution have a side-effect of setting conditional bits in the status register
	* Subsequent instructions can become conditional on the bases of these set bits 

### Thumb control flow
* Fairly standard: 
	* Unconditional branches (BU)
	* Conditional branches (BC)
	* Calls (BL)
* Control flow changes cause a pipeline flush and a execution time penalty
* However, the limited instruction width places a restriction on how long the reachable distances are
* The BL (Branch with link) instruction is unusual, it needs greater than 16 bits to encode the jump, so it is formed of two instructions that are concatenated by hardware

---

## Assemblers and assembly language

### What is assembly language?
* Low level programming language (close to hardware)
* Normally separate ones for each target processor architecture
* Consists of mnemonics and explicit location and argument information
* Generally the ratio of assembly instructions to architecture instructions (native) is 1:1

### Assemblers 
* Assembly language is the input to a program called an assembler
* Assemblers take assembly language as input and produce architecture-specific machine code as output
* Machine code is the binary language that is directly executable on the processor

### Why not code in assembly?
It used to be the most popular language, before high-level languages took over, but now it is rarely coded in for the following reasons:
	* Assembly code is mostly machine-specific
	* Assembly is generally too low-level for program writing (one line of C vs many lines of assembly)
	* A human should not have to insert labels, addresses and immediates by hand
	* Readability (and maintainability) is very poor for large segments of assembly

### When is it useful?
* Understanding the hardware and what is going on
* Understanding the timing and requirements of code fragments (e.g. real time systems)
* Designing compact executables for systems where memory is important 
* Performing optimisations in code sections that are commonly used
* Interacting with normally hidden architectural aspects

### What does an assembler look like?
* Code is of of the form ADD r0, r1, r2 or ADD r0, #1 (from ARM thumb syntax)

### Labels
* Labels are the way of naming code locations in assembler
* e.g. in ARM syntax:

```
.loop			<- label
	ADD r0, #1
	SUB r1, #1
	BNE loop
```

* This works because the assembler translates the label into a real address (usually we would not be able to pass text to the assembler)
* The assembler completes the translation in a method called the two-step method, which is particularly important for forward references 

### Two-pass assembling
* Sweep through the program to break each line into its components (opcodes, operands, condition codes, etc). Keep track of instruction sizes (easy in fixed length ISAs). If a label is found store it in a table along with memory address
* Starting again at the beginning of program, now convert each line to binary machine code, using values in the table.

### The thumb assembler - general format
* Thumb instructions take an instruction and 0 - 3 arguments. These arguments may be registers, immediates or addresses

### Thumb terminology
Mnemonics are used in assembler instructions
* rm: 1st source argument
* rn: 2nd source argument
* rd: result destination
* rdn: result destination and 1st source
* i: immediate

### Examples
1. Add with immediate (implicit first reg)
	* ADD rm, i8
	* e.g. ADD r0, #3
2. Add registers
	* ADD rd, rm, rn
	* e.g. ADD r0, r1, r2
3. ADD to stack pointer and update it
	* ADD sp, i8
	* e.g. ADD sp, #5
4. ADD SP/PC and immediate, place in register
	* ADD rd, pc, i8
	* e.g. ADD r0, pc, #12

### Zoom in on arguments
* Various instruction arguments must be specified:
	* Immediate: preceded by '#'
	* Register, by name (e.g. 'r0')
	* Shifts (e.g. LSL #2 to shift left two)
	* Addresses, surrounded by square brackets: [r1] means treat the contents of r1 as an address whereas [44] means access address 44
* Argument length differs from instruction to instruction
* Registers: either implicit or 3-4 bits long
* Immediates: from 5-23 bits long

### Memory instructions
* Thumb v1 has two basic and some specialised instruction
* Load: LDR \<dest reg>, [\<address>] e.g LDR r0, [r1] loads the contents of memory at r1's address into r0
* Store: STR \<source reg>, [\<address>]
* e.g. STR r0, [r1] stores the contents of r0 into r1's address
* PUSH and POP: dedicated instructions for efficient stack access
* Implicit destination and source in Thumb v1
* Can take multiple register arguments: PUSH {r0, r1, r2} pushes 3 register values onto the stack

### Assembler translation
* When running over an input assembly file, a key feature of an assembler is to map the input to the 'best' output instruction (i.e select the closest match or the most efficient)
* Sometimes the argument length is considered
* For example, there are multiple different formatting options for the ADD instruction - the choice of format decided on will be determined from the arguments (as long as it is still adding it does not matter which one is used)

### Assembly directives
* Sometimes called a psuedo opcode
* Is a piece of assembly that does not get converted into machine code
* Tells the assembler to perform some action
* There are multiple types of directives:
	* Data directives tell the assembler to reserve spaces in the program for data e.g. SPACE 2 ; reserves two bytes of memory
	* Symbol directives define symbols for ease of use in programming the assembly e.g. apple RN r6; defines "apple" for register 6

---


