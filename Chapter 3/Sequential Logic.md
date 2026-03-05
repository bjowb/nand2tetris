
### Combinational Logic

- Combinational chips compute functions that depend solely on combinations
	of their input values.	
- These relatively simple chips provide many important processing functions (like the ALU), but they cannot maintain state.

- Since computers must be able to not only compute values but also store and recall values, they must be equipped with memory elements that can preserve data over time. 

 
 These memory elements are built from **sequential chips**.

### Sequential Logic 

- The implementation of memory elements is an intricate art involving synchronization, clocking, and feedback loops. 

- Conveniently, most of this complexity can be embedded in the operating logic of very low-level sequential gates called ﬂip-ﬂops.

- Using these ﬂip-ﬂops as elementary building blocks, we will specify and build all
	the memory devices employed by typical modern computers, from binary cells to
	registers to memory banks and counters.


### Components

#### Clock

- In most computers, the passage of time is represented by a master clock
	that delivers a continuous train of alternating signals.


- The exact hardware implementation is usually based on an oscillator that alternates continuously between two phases labeled 0–1, low-high, tick-tock, etc.

- The elapsed time between the beginning of a ‘‘tick’’ and the end of the subsequent ‘‘tock’’ is called cycle, and each clock cycle is taken to model one discrete time unit.

- The current clock phase (tick or tock) is represented by a binary signal. Using the hardware’s circuitry, this signal is simultaneously broadcast to every sequential chip throughout the computer platform.

#### Flip - Flops

- The most elementary sequential element in the computer is a device called a ﬂip-ﬂop, of which there are several variants.

- In this book we use a variant called a data ﬂip-ﬂop, or DFF, whose interface consists of a single-bit data inputand a single-bit data output.

- In addition, the DFF has a clock input that continuously changes according to the master clock’s signal. Taken together, the data and the clock inputs enable the DFF to implement the time-based behavior outðtÞ ¼ inðt  1Þ, where in and out are the gate’s input and output values and t is the current clock cycle.

- In other words, the DFF simply outputs the input value from the previous time unit.

- As we now show, this elementary behavior can form the basis of all the hardware devices that computers use to maintain state, from binary cells to registers to arbitrarily large random access memory (RAM) units.

#### Registers

- A register is a storage device that can ‘‘store,’’ or ‘‘remember,’’ a value
over time, implementing the classical storage behavior outðtÞ ¼ outðt  1Þ.

- A DFF,on the other hand, can only output its previous input, namely, outðtÞ ¼ inðt  1Þ.

- This suggests that a register can be implemented from a DFF by simply feeding the
	output of the latter back into its input, creating the device shown in the middle of
	ﬁgure 3.1. Presumably, the output of this device at any time t will echo its output at
	time t  1, yielding the classical function expected from a storage unit.

![[Pasted image 20260305194436.png]]

![[Pasted image 20260305194603.png]]

#### Memory

- Once we have the basic ability to represent words, we can proceed to
build memory banks of arbitrary length.

- As [[Pasted image 20260305194631.png]] shows, this can be done by stacking together many registers to form a Random Access Memory RAM unit.
- The term random access memory derives from the requirement that read/write operations n a RAM should be able to access randomly chosen words, with no restrictions on the order in which they are accessed.
- That is to say, we require that any word in the memory—irrespective of its physical location—be accessed directly, in equal speed.

This requirement can be satisﬁed as follows :
1) we assign each word in the n-register RAM a unique address (an integer between 0 to n  1), according to which it will be accessed.
2) in addition to building an array of n registers, we build a gate logic design that, given an address j, is capable of selecting the individual register whose address is j.

###### What it actually does ?
- A classical RAM device accepts three inputs:
1) a data input
2) an address input
3) a load bit. 

- The address speciﬁes which RAM register should be accessed in the current time unit. In the case of a read operation (load=0), the RAM’s output immediately emits the value of the selected register. 
- In the case of a write operation (load=1), the selected memory register commits to the input value in the next time unit, at which point the RAM’s output will start emitting it.

- ![[Pasted image 20260305194631.png]]


#### Counters

- A counter is a sequential chip whose state is an integer number that
	increments every time unit, effecting the function outðtÞ ¼ outðt  1Þ þ c, where c is
	typically 1.
- Counters play an important role in digital architectures. 
- For example, a typical CPU includes a program counter whose output is interpreted as the address of the instruction that should be executed next in the current program.

- A counter chip can be implemented by combining the input/output logic of a
	standard register with the combinatorial logic for adding a constant. Typically, the
	counter will have to be equipped with some additional functionality, such as possi-
	bilities for resetting the count to zero, loading a new counting base, or decrementing
	instead of incrementing.

![[Pasted image 20260305195644.png]]

#### Time matters

- All the chips described so far in this chapter are sequential. 
- Simply stated, a sequential chip is a chip that embeds one or more DFF gates, either directly or indirectly.
- Functionally speaking, the DFF gates endow sequential chips with the ability to either maintain state (as in memory units) or operate on state Technically speaking, this is done by forming feedback loops inside the sequential chip (see ﬁgure 3.4). 

- In combinational chips, where time is neither modeled nor recognized, the introduction of feedback loops is problematic:
	The output would depend on the input, which itself would depend on the output, and thus the output would depend on itself.
	
- On the other hand, there is no difﬁculty in feeding the output of a sequential chip back into itself, since the DFFs introduce an inherent time delay:
	The output at time t does not depend on itself, but rather on the output at
	time t  1. This property guards against the uncontrolled ‘‘data races’’ that would
	occur in combinational chips with feedback loops.

- Recall that the outputs of combinational chips change when their inputs change, irrespective of time. 
- In contrast, the inclusion of the DFFs in the sequential architecture ensures that their outputs change only at the point of transition from one clock cycle to the next, and not within the cycle itself. 
- In fact, we allow sequential chips to be in unstable states during clock cycles, requiring only that at the beginning of the next cycle they output correct values.
- This ‘‘discretization’’ of the sequential chips’ outputs has an important side effect:
	It can be used to synchronize the overall computer architecture. To illustrate, sup-
	pose we instruct the arithmetic logic unit (ALU) to compute x þ y where x is the
	value of a nearby register and y is the value of a remote RAM register. Because of
	various physical constraints (distance, resistance, interference, random noise, etc.) the
	electric signals representing x and y will likely arrive at the ALU at different times.

- However, being a combinational chip, the ALU is insensitive to the concept of time—
	it continuously adds up whichever data values happen to lodge in its inputs. Thus, it
	will take some time before the ALU’s output stabilizes to the correct x þ y result.
	Until then, the ALU will generate garbage.

#### How can we overcome this difﬁculty? 

- Well, since the output of the ALU is always
	routed to some sort of a sequential chip (a register, a RAM location, etc.), we don’t
	really care. All we have to do is ensure, when we build the computer’s clock, that
	the length of the clock cycle will be slightly longer that the time it takes a bit to travel
	the longest distance from one chip in the architecture to another. This way, we are
	guaranteed that by the time the sequential chip updates its state (at the beginning of
	the next clock cycle), the inputs that it receives from the ALU will be valid. This, in a
	nutshell, is the trick that synchronizes a set of stand-alone hardware components into
	a well-coordinated system.

### Implementation

#### Data Flip Flops

![[Pasted image 20260305200303.png]]

```
CHIP Bit {
    IN in, load;
    OUT out;

    PARTS:
    Mux(a= dff1, b=in , sel=load , out=dff );
    DFF(in= dff, out=dff1,out = out);
}
```

#### Registers

![[Pasted image 20260305200807.png]]

```
CHIP Register {
    IN in[16], load;
    OUT out[16];

    PARTS:
    //// Replace this comment with your code.
    Bit(in=in[0], load=load, out=out[0]);
    Bit(in=in[1], load=load, out=out[1]);
    Bit(in=in[2], load=load, out=out[2]);
    Bit(in=in[3], load=load, out=out[3]);
    Bit(in=in[4], load=load, out=out[4]);
    Bit(in=in[5], load=load, out=out[5]);
    Bit(in=in[6], load=load, out=out[6]);
    Bit(in=in[7], load=load, out=out[7]);
    Bit(in=in[8], load=load, out=out[8]);
    Bit(in=in[9], load=load, out=out[9]);
    Bit(in=in[10], load=load, out=out[10]);
    Bit(in=in[11], load=load, out=out[11]);
    Bit(in=in[12], load=load, out=out[12]);
    Bit(in=in[13], load=load, out=out[13]);
    Bit(in=in[14], load=load, out=out[14]);
    Bit(in=in[15], load=load, out=out[15]);
}
```

#### Memory
![[Pasted image 20260305201342.png]]

```
CHIP RAM8 {
    IN in[16], load, address[3];
    OUT out[16];

    PARTS:
    DMux8Way(in=load, sel=address, a=loada, b=loadb, c=loadc,
            d=loadd, e=loade, f=loadf, g=loadg, h=loadh);
    
    Register(in=in, load=loada, out=outa);
    Register(in=in, load=loadb, out=outb);
    Register(in=in, load=loadc, out=outc);
    Register(in=in, load=loadd, out=outd);
    Register(in=in, load=loade, out=oute);
    Register(in=in, load=loadf, out=outf);
    Register(in=in, load=loadg, out=outg);
    Register(in=in, load=loadh, out=outh);

    Mux8Way16(a=outa, b=outb, c=outc, d=outd, e=oute,
            f=outf, g=outg, h=outh, sel=address, out=out);
}

CHIP RAM64 {
    IN in[16], load, address[6];
    OUT out[16];

    PARTS:
    DMux8Way(in=load, sel=address[3..5], a=loada, b=loadb, c=loadc,
            d=loadd, e=loade, f=loadf, g=loadg, h=loadh);
    
    RAM8(in=in, load=loada, address=address[0..2], out=outa);
    RAM8(in=in, load=loadb, address=address[0..2], out=outb);
    RAM8(in=in, load=loadc, address=address[0..2], out=outc);
    RAM8(in=in, load=loadd, address=address[0..2], out=outd);
    RAM8(in=in, load=loade, address=address[0..2], out=oute);
    RAM8(in=in, load=loadf, address=address[0..2], out=outf);
    RAM8(in=in, load=loadg, address=address[0..2], out=outg);
    RAM8(in=in, load=loadh, address=address[0..2], out=outh);

    Mux8Way16(a=outa, b=outb, c=outc, d=outd, e=oute,
            f=outf, g=outg, h=outh, sel=address[3..5], out=out);
}


CHIP RAM512 {
    IN in[16], load, address[9];
    OUT out[16];

    PARTS:
    DMux8Way(in=load, sel=address[6..8], a=loada, b=loadb, c=loadc,
            d=loadd, e=loade, f=loadf, g=loadg, h=loadh);
    
    RAM64(in=in, load=loada, address=address[0..5], out=outa);
    RAM64(in=in, load=loadb, address=address[0..5], out=outb);
    RAM64(in=in, load=loadc, address=address[0..5], out=outc);
    RAM64(in=in, load=loadd, address=address[0..5], out=outd);
    RAM64(in=in, load=loade, address=address[0..5], out=oute);
    RAM64(in=in, load=loadf, address=address[0..5], out=outf);
    RAM64(in=in, load=loadg, address=address[0..5], out=outg);
    RAM64(in=in, load=loadh, address=address[0..5], out=outh);

    Mux8Way16(a=outa, b=outb, c=outc, d=outd, e=oute,
            f=outf, g=outg, h=outh, sel=address[6..8], out=out);
}

CHIP PC {
    IN in[16], load, inc, reset;
    OUT out[16];

    PARTS:
    Inc16(in=oldVal, out=newVal);
    
    Mux16(a=false, b=newVal, sel=inc, out=incRes);
    Mux16(a=incRes, b=in, sel=load, out=loadRes);
    Mux16(a=loadRes, b=false, sel=reset, out=resetRes);

    Or(a=load, b=reset, out=ora);
    Or(a=ora, b=inc, out=changes);

    // If we didn't have any changes, load is 0 and therefore we return oldVal.
    // Otherwise, we update to resetRes, which has the updated value after inc, load and reset.
    Register(in=resetRes, load=changes, out=oldVal, out=out);
}
```