
 - Our starting point is the set of logic gates built in chapter 1, and our ending point is a fully functional Arithmetic Logical Unit.
- ALU is the centerpiece chip that executes all the arithmetic and logical operations performed by the computer.

#### Binary Numbers

Unlike the decimal system, which is founded on base 10, the binary system is founded on base 2.

![[Pasted image 20260303165531.png]]

![[Pasted image 20260303165543.png]]

#### Binary Addition

A pair of binary numbers can be added digit by digit from right to left, according to the same elementary school method used in decimal addition.

First,
we add the two right-most digits, also called the least signiﬁcant bits (LSB) of the two
binary numbers. Next, we add the resulting carry bit (which is either 0 or 1) to the
sum of the next pair of bits up the signiﬁcance ladder. 
We continue the process until
the two most signiﬁcant bits (MSB) are added. If the last bit-wise addition generates a
carry of 1, we can report overﬂow; otherwise, the addition completes successfully:

![[Pasted image 20260303165635.png]]

#### Signed Binary Numbers

A binary system with n digits can generate a set of 2 n dif-
ferent bit patterns. If we have to represent signed numbers in binary code, a natural
solution is to split this space into two equal subsets. One subset of codes is assigned
to represent positive numbers, and the other negative numbers.

The method used today by almost all computers is called the 2’s complement method, also known as radix complement.

![[Pasted image 20260303165733.png]]

For example, in a 5-bit binary system, the 2’s complement representation of 2
or ‘‘minusð00010Þtwo ’’ is 2 5  ð00010Þtwo ¼ ð32Þten  ð2Þten ¼ ð30Þten ¼ ð11110Þtwo . To
check the calculation, the reader can verify that ð00010Þtwo þ ð11110Þtwo ¼ ð00000Þtwo .
Note that in the latter computation, the sum is actually ð100000Þtwo , but since we are
dealing with a 5-bit binary system, the left-most sixth bit is simply ignored. As a rule,
when the 2’s complement method is applied to n-bit numbers, x þ ðxÞ always sums
up to 2 n (i.e., 1 followed by n 0’s)—a property that gives the method its name. Figure
2.1 illustrates a 4-bit binary system with the 2’s complement method.

![[Pasted image 20260303165802.png]]



### Adders

We present a hierarchy of three adders, leading to a multi-bit adder chip:
-  Half-adder: designed to add two bits
- Full-adder: designed to add three bits
- Adder: designed to add two n-bit numbers

##### Half Adder
![[Pasted image 20260303170010.png]]

```
CHIP HalfAdder {
    IN a, b;    // 1-bit inputs
    OUT sum,    // Right bit of a + b 
        carry;  // Left bit of a + b

    PARTS:
    Xor(a= a, b= b, out= sum);
    And(a= a, b= b, out= carry);
}
```

#### Full Adder
![[Pasted image 20260303170502.png]]

```
CHIP FullAdder {
    IN a, b, c;  // 1-bit inputs
    OUT sum,     // Right bit of a + b + c
        carry;   // Left bit of a + b + c

    PARTS:
    HalfAdder(a=a, b=b, sum=absum, carry=abcarry);
    HalfAdder(a=absum, b=c, sum=sum, carry=abccarry);
    Or(a=abcarry, b=abccarry, out=carry);
}
```

#### Add16
![[Pasted image 20260305190331.png]]

```
CHIP Add16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
    FullAdder(a=a[0], b=b[0], c=false, sum=out[0], carry=carry0);
    FullAdder(a=a[1], b=b[1], c=carry0, sum=out[1], carry=carry1);
    FullAdder(a=a[2], b=b[2], c=carry1, sum=out[2], carry=carry2);
    FullAdder(a=a[3], b=b[3], c=carry2, sum=out[3], carry=carry3);
    FullAdder(a=a[4], b=b[4], c=carry3, sum=out[4], carry=carry4);
    FullAdder(a=a[5], b=b[5], c=carry4, sum=out[5], carry=carry5);
    FullAdder(a=a[6], b=b[6], c=carry5, sum=out[6], carry=carry6);
    FullAdder(a=a[7], b=b[7], c=carry6, sum=out[7], carry=carry7);
    FullAdder(a=a[8], b=b[8], c=carry7, sum=out[8], carry=carry8);
    FullAdder(a=a[9], b=b[9], c=carry8, sum=out[9], carry=carry9);
    
    FullAdder(a=a[10], b=b[10], c=carry9, sum=out[10], carry=carry10);
    FullAdder(a=a[11], b=b[11], c=carry10, sum=out[11], carry=carry11);
    FullAdder(a=a[12], b=b[12], c=carry11, sum=out[12], carry=carry12);
    FullAdder(a=a[13], b=b[13], c=carry12, sum=out[13], carry=carry13);
    FullAdder(a=a[14], b=b[14], c=carry13, sum=out[14], carry=carry14);
    FullAdder(a=a[15], b=b[15], c=carry14, sum=out[15], carry=carry15);
}
```


### ALU

- The Hack ALU computes a ﬁxed set of functions out ¼ fi ðx; yÞ where x and y are
	the chip’s two 16-bit inputs, out is the chip’s 16-bit output, and fi is an arithmetic
	or logical function selected from a ﬁxed repertoire of eighteen possible functions.

- We instruct the ALU which function to compute by setting six input bits, called control
	bits, to selected binary values.

- ![[Pasted image 20260305191002.png]]

- ![[Pasted image 20260305191059.png]]

```
CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute out = x + y (if 1) or x & y (if 0)
        no; // negate the out output?

    OUT 
        out[16], // 16-bit output
        zr, // 1 if (out == 0), 0 otherwise
        ng; // 1 if (out < 0),  0 otherwise

    PARTS:
    // Handling x
    Mux16(a=x, b=false, sel=zx, out=xAfterZero);                 // zx == 1 => use Mux to get {0,...,0}. Otherwise, keep the same

    Not16(in=xAfterZero, out=notXAfterZero);                     // Calculate the opposite of the result of previous stage
    Mux16(a=xAfterZero, b=notXAfterZero, sel=nx, out=xAfterAll); // nx == 1 => use Mux to get the opposite. Otherwise, keep the same

    // Same with y
    Mux16(a=y, b=false, sel=zy, out=yAfterZero);

    Not16(in=yAfterZero, out=notYAfterZero);
    Mux16(a=yAfterZero, b=notYAfterZero, sel=ny, out=yAfterAll);

    // if f == 1
    Add16(a=xAfterAll, b=yAfterAll, out=xAddY);
    And16(a=xAfterAll, b=yAfterAll, out=xAndY);
    Mux16(a=xAndY, b=xAddY, sel=f, out=xFY);

    // handling 'no'
    Not16(in=xFY, out=notXFY);
    Mux16(a=xFY, b=notXFY, sel=no, out=out, out[0..7]=outFirst, out[8..15]=outSecond, out[15]=ng);

    // Taking care of stat (zr)
    Or8Way(in=outFirst, out=or1);  // If we have a single '1' in 1st half
    Or8Way(in=outSecond, out=or2); // If we have a single '1' in 2nd half
    Or(a=or1, b=or2, out=isZr);    // If we have a single '1' in all output
    Mux(a=true, b=false, sel=isZr, out=zr); // if we don't have any 1's, we set zr to be 1
}
```