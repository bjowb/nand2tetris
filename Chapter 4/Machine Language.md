
- we complete the construction of a general-purpose computer designed to run machine
	language programs.

- A machine language is an agreed-upon formalism, designed to code low-level
	programs as series of machine instructions. 
	Using these instructions, the programmer can command the processor to perform arithmetic and logic operations, fetch and store values from and to the memory, move values from one register to another, test Boolean conditions, and so on.
	As opposed to high-level languages, whose basic design goals are generality and power of expression, the goal of machine language’s design is direct execution in, and total control of, a given hardware platform. 
	Of course, generality, power, and elegance are still desired, but only to the extent that
	they support the basic requirement of direct execution in hardware.

- Machine language is the most profound interface in the overall computer
	enterprise—the ﬁne line where hardware and software meet. 
	This is the point where the abstract thoughts of the programmer, as manifested in symbolic instructions, are turned into physical operations performed in silicon.


### Terminologies

#### Machine

- A machine language can be viewed as an agreed-upon formalism, designed to manipulate a memory using a processor and a set of registers.

##### Memory

- The term memory refers loosely to the collection of hardware devices that
	store data and instructions in a computer.

##### Processsor

- The processor, normally called Central Processing Unit or CPU, is a
	device capable of performing a ﬁxed set of elementary operations.
	 These typically include arithmetic and logic operations, memory access operations, and control (also called branching) operations.

##### Registers

- Memory access is a relatively slow operation, requiring long instruction formats (an address may require 32 bits).
	For this reason, most processors are equipped with several registers, each capable of holding a single value. 
	Located in the processor’s immediate proximity, the registers serve as a high-speed local memory, allowing the processor to manipulate data and instructions quickly.
	This setting enables the programmer to minimize the use of memory access commands, thus speeding up the program’s execution.

#### Language

- A machine language program is a series of coded instructions
- For example, a typical instruction in a 16-bit computer may be 1010001100011001. In order to ﬁgure out what this instruction means, we have to know the rules of the game, namely, the instruction set of the underlying hardware platform.

- Machine languages are normally speciﬁed using both binary codes and symbolic mnemonics (a mnemonic is a symbolic label whose name hints at what it stands for—in our case hardware elements and binary operations).
- we can use a text processing program to parse the symbolic commands into their underlying ﬁelds (mnemonics and operands), translate each ﬁeld into its equivalent binary representation, and assemble the resulting codes into binary machine instructions. 
- The symbolic notation is called assembly language, or simply assembly, and the program that translates from assembly to binary is called assembler.

#### Commands

##### Arithmetic and Logic Operations
- Every computer is required to perform basic
	arithmetic operations like addition and subtraction as well as basic Boolean oper-
	ations like bit-wise negation, bit shifting, and so forth.

![[Pasted image 20260308135847.png]]

##### Memory Access

- Memory access commands fall into two categories :
1) as we
	have just seen, arithmetic and logical commands are allowed to operate not only on
	registers, but also on selected memory locations. 
2) all computers feature explicit load and store commands, designed to move data between registers and memory.

	These memory access commands may use several types of addressing modes—
	ways of specifying the address of the required memory word.

ADDRESSING MODES :

1) Direct addressing The most common way to address the memory is to express a
	speciﬁc address or use a symbol that refers to a speciﬁc address,

![[Pasted image 20260308140149.png]]
2) Immediate addressing This form of addressing is used to load constants—
	namely, load values that appear in the instruction code: Instead of treating the nu-
	meric ﬁeld that appears in the instruction as an address, we simply load the value of
	the ﬁeld itself into the register,

![[Pasted image 20260308140239.png]]

3) Indirect addressing In this addressing mode the address of the required memory
	location is not hard-coded into the instruction; instead, the instruction speciﬁes a
	memory location that holds the required address.
	 This addressing mode is used to handle *pointers*.
![[Pasted image 20260308140410.png]]

##### Flow Control
![[Pasted image 20260308142220.png]]

### Specifications

#### Overview
- The Hack computer is a von Neumann platform.
- It is a 16-bit machine, consisting of a CPU, two separate memory modules serving as instruction memory and data memory, and two memory-mapped I/O devices: a screen and a keyboard.

##### Memory Address Space
- The Hack programmer is aware of two distinct address
	spaces: an instruction memory and a data memory. Both memories are 16-bit wide
	and have a 15-bit address space, meaning that the maximum addressable size of each
	memory is 32K 16-bit words.

- The CPU can only execute programs that reside in the instruction memory. The
	instruction memory is a read-only device, and programs are loaded into it using some
	exogenous means.

##### Register
- The Hack programmer is aware of two 16-bit registers called D and A.
- Every operation involving a memory location
	requires two Hack commands:
	One for selecting the address on which we want to operate, and one for specifying the desired operation. 
	Indeed, the Hack language consists of two generic instructions: an address instruction, also called A-instruction, and 
	a compute instruction, also called C-instruction.
	Each instruction has a binary representation, a symbolic representation, and an effect on the computer, as we now specify.

#### The A-Instruction
- The A-instruction is used to set the A register to a 15-bit value:

![[Pasted image 20260308155032.png]]

- For example, the instruction @5, which is equivalent to 0000000000000101, causes the
	computer to store the binary representation of 5 in the A register.

![[Pasted image 20260308155653.png]]

#### The C-Instruction
- The C-instruction is the programming workhorse of the Hack platform—the instruction that gets almost everything done.
- The instruction code is a speciﬁcation
	that answers three questions: 
	(a) what to compute
	(b) where to store the computed value 
	(c) what to do next?

![[Pasted image 20260308160029.png]]

- The overall semantics of the symbolic instruction destn. comp;jump is as follows. The comp ﬁeld instructs the ALU what to compute.
- The dest ﬁeld instructs where to store the computed value (ALU output). 
- The jump ﬁeld speciﬁes a jump condition, namely, which command to fetch and execute
	next. We now describe the format and semantics of each of the three ﬁelds.


