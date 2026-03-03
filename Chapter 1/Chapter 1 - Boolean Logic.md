
### Elementary Logic Gates

- The gates can be physically implemented in many different materials and fabrication technologies, but their logical behavior is consistent across all computers.

### Boolean Algebra

- Boolean algebra deals with Boolean (also called binary) values that are typically labeled true/false, 1/0, yes/no, on/off, and so forth. 
- We will use 1 and 0. 
- A Boolean function is a function that operates on binary inputs and returns binary outputs.

#### Truth Table
- simplest way to specify a Boolean function is to enumerate all the possible values of the function’s input variables, along with the function’s output for each set of inputs. This is called the truth table representation
- Example :
- the function defined in figure 1.1 is equivalently given by the :
![[Pasted image 20260222190013.png]]

 - ![[Pasted image 20260222190052.png]]

### Gate

- A gate is a physical device that implements a Boolean function
-  If a Boolean function f operates on n variables and returns m binary results (in all our examples so far, m was 1), the gate that implements f will have n input pins and m output pins.
-  The simplest gates of all are made from tiny switching devices, called transistors, wired in a certain topology designed to effect the overall gate functionality.
- a primitive gate (see figure 1.3) can be viewed as a black box device that implements an elementary logical operation in one way or another—we don’t care how. 
- hardware designer starts from such primitive gates and designs more complicated functionality by interconnecting them, leading to the construction of composite gates.
- ![[Pasted image 20260222220612.png]]

### Basic Logic Gates

##### Not Gate
![[Pasted image 20260302210327.png]]

```
CHIP Not {
    IN in;
    OUT out;

    PARTS:
    Nand(a= in, b= in, out= out);
}
```

##### And Gate

![[Pasted image 20260302210620.png]]

```
CHIP And {
    IN a, b;
    OUT out;
    
    PARTS:
    Nand(a=a , b=b , out=out1 );
    Not(in= out1, out= out);
}
```

##### Or Gate
![[Pasted image 20260302210639.png]]

```
CHIP Or {
    IN a, b;
    OUT out;

    PARTS:
    Not(in=a , out=a1 );
    Not(in=b , out=b1 );

    And(a=a1 , b=b1 , out=out1 );
    Not(in=out1 , out=out );
} 
```

##### Xor Gate
![[Pasted image 20260302210703.png]]

```
CHIP Xor {
    IN a, b;
    OUT out;

    PARTS:
    Not(in=a , out=a1 );
    Not(in = b, out = b1);

    And(a=a , b=b1 , out=out1 );
    And(a=a1 , b=b , out=out2 );

    Or(a=out1 , b=out2 , out=out );
}
```

##### Multiplexer Gate
![[Pasted image 20260302210729.png]]

```
CHIP Mux {
    IN a, b, sel;
    OUT out;

    PARTS:
    Not(in=sel , out=sel1 );

    And(a=a , b=sel1 , out=out1 );
    And(a=b , b=sel , out=out2 );

    Or(a=out1 , b=out2 , out=out );
}
```


##### De-multiplexer Gate
![[Pasted image 20260302210810.png]]

```
CHIP DMux {
    IN in, sel;
    OUT a, b;

    PARTS:
    Not(in= sel, out= seln);

    And(a= in, b= seln, out= a);
    And(a= in, b= sel, out= b);
}
```

##### Multi-bit Not
![[Pasted image 20260303160555.png]]

```
CHIP Not16 {
    IN in[16];
    OUT out[16];

    PARTS:
    Not(in= in[0], out= out[0]);
    Not(in= in[1], out= out[1]);
    Not(in= in[2], out= out[2]);
    Not(in= in[3], out= out[3]);
    Not(in= in[4], out= out[4]);
    Not(in= in[5], out= out[5]);
    Not(in= in[6], out= out[6]);
    Not(in= in[7], out= out[7]);
    Not(in= in[8], out= out[8]);
    Not(in= in[9], out= out[9]);
    Not(in= in[10], out= out[10]);
    Not(in= in[11], out= out[11]);
    Not(in= in[12], out= out[12]);
    Not(in= in[13], out= out[13]);
    Not(in= in[14], out= out[14]);
    Not(in= in[15], out= out[15]);
    
}
```

##### Multi Bit AND
![[Pasted image 20260303160621.png]]

```
CHIP And16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
    And(a=a[0], b=b[0], out=out[0]);
    And(a=a[1], b=b[1], out=out[1]);
    And(a=a[2], b=b[2], out=out[2]);
    And(a=a[3], b=b[3], out=out[3]);
    And(a=a[4], b=b[4], out=out[4]);
    And(a=a[5], b=b[5], out=out[5]);
    And(a=a[6], b=b[6], out=out[6]);
    And(a=a[7], b=b[7], out=out[7]);
    And(a=a[8], b=b[8], out=out[8]);
    And(a=a[9], b=b[9], out=out[9]);
    And(a=a[10], b=b[10], out=out[10]);
    And(a=a[11], b=b[11], out=out[11]);
    And(a=a[12], b=b[12], out=out[12]);
    And(a=a[13], b=b[13], out=out[13]);
    And(a=a[14], b=b[14], out=out[14]);
    And(a=a[15], b=b[15], out=out[15]);
}
```

##### Multi Bit OR
![[Pasted image 20260303160704.png]]

```
CHIP Or16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
    Or(a=a[0], b=b[0], out=out[0]);
    Or(a=a[1], b=b[1], out=out[1]);
    Or(a=a[2], b=b[2], out=out[2]);
    Or(a=a[3], b=b[3], out=out[3]);
    Or(a=a[4], b=b[4], out=out[4]);
    Or(a=a[5], b=b[5], out=out[5]);
    Or(a=a[6], b=b[6], out=out[6]);
    Or(a=a[7], b=b[7], out=out[7]);
    Or(a=a[8], b=b[8], out=out[8]);
    Or(a=a[9], b=b[9], out=out[9]);
    Or(a=a[10], b=b[10], out=out[10]);
    Or(a=a[11], b=b[11], out=out[11]);
    Or(a=a[12], b=b[12], out=out[12]);
    Or(a=a[13], b=b[13], out=out[13]);
    Or(a=a[14], b=b[14], out=out[14]);
    Or(a=a[15], b=b[15], out=out[15]);
}
```

##### Multi Bit Multiplexer
![[Pasted image 20260302210810.png]]

```
CHIP Mux16 {
    IN a[16], b[16], sel;
    OUT out[16];

    PARTS:
    Mux(a=a[0], b=b[0], sel=sel, out=out[0]);
    Mux(a=a[1], b=b[1], sel=sel, out=out[1]);
    Mux(a=a[2], b=b[2], sel=sel, out=out[2]);
    Mux(a=a[3], b=b[3], sel=sel, out=out[3]);
    Mux(a=a[4], b=b[4], sel=sel, out=out[4]);
    Mux(a=a[5], b=b[5], sel=sel, out=out[5]);
    Mux(a=a[6], b=b[6], sel=sel, out=out[6]);
    Mux(a=a[7], b=b[7], sel=sel, out=out[7]);
    Mux(a=a[8], b=b[8], sel=sel, out=out[8]);
    Mux(a=a[9], b=b[9], sel=sel, out=out[9]);
    Mux(a=a[10], b=b[10], sel=sel, out=out[10]);
    Mux(a=a[11], b=b[11], sel=sel, out=out[11]);
    Mux(a=a[12], b=b[12], sel=sel, out=out[12]);
    Mux(a=a[13], b=b[13], sel=sel, out=out[13]);
    Mux(a=a[14], b=b[14], sel=sel, out=out[14]);
    Mux(a=a[15], b=b[15], sel=sel, out=out[15]);
}
```

##### Multi Bit DeMultiplexer
![[Pasted image 20260302210810.png]]

```
CHIP DMux {
    IN in, sel;
    OUT a, b;

    PARTS:
    Not(in= sel, out= seln);

    And(a= in, b= seln, out= a);
    And(a= in, b= sel, out= b);
}
```

#### OR 8 way
![[Pasted image 20260303164115.png]]


```
CHIP Or8Way {
    IN in[8];
    OUT out;

    PARTS:
    Or(a=in[0], b=in[1], out=ab);
    Or(a=in[2], b=in[3], out=cd);
    Or(a=in[4], b=in[5], out=ef);
    Or(a=in[6], b=in[7], out=gh);

    Or(a=ab, b=cd, out=abcd);
    Or(a=ef, b=gh, out=efgh);

    Or(a=abcd, b=efgh, out=out);
}
```

#### MUX 4/8 way 16

![[Pasted image 20260303164237.png]]

```
CHIP Mux4Way16 {
    IN a[16], b[16], c[16], d[16], sel[2];
    OUT out[16];

    PARTS:
    Mux16(a=a, b=b, sel=sel[0], out=ab);
    Mux16(a=c, b=d, sel=sel[0], out=cd);
    
    Mux16(a=ab, b=cd, sel=sel[1], out=out);
}

CHIP Mux8Way16 {
    IN a[16], b[16], c[16], d[16],
       e[16], f[16], g[16], h[16],
       sel[3];
    OUT out[16];

    PARTS:
    Mux16(a=a, b=b, sel=sel[0], out=ab);
    Mux16(a=c, b=d, sel=sel[0], out=cd);
    Mux16(a=e, b=f, sel=sel[0], out=ef);
    Mux16(a=g, b=h, sel=sel[0], out=gh);
    
    Mux16(a=ab, b=cd, sel=sel[1], out=abcd);
    Mux16(a=ef, b=gh, sel=sel[1], out=efgh);

    Mux16(a=abcd, b=efgh, sel=sel[2], out=out);
}
```

### DeMux 4/8 way 16
![[Pasted image 20260303164438.png]]

```
CHIP DMux4Way {
    IN in, sel[2];
    OUT a, b, c, d;

    PARTS:
    DMux(in=in, sel=sel[1], a=a2b, b=c2d);

    DMux(in=a2b, sel=sel[0], a=a, b=b);
    DMux(in=c2d, sel=sel[0], a=c, b=d);
}

CHIP DMux8Way {
    IN in, sel[3];
    OUT a, b, c, d, e, f, g, h;

    PARTS:
    DMux(in=in, sel=sel[2], a=a2d, b=e2h);

    DMux(in=a2d, sel=sel[1], a=a2b, b=c2d);
    DMux(in=e2h, sel=sel[1], a=e2f, b=g2h);

    DMux(in=a2b, sel=sel[0], a=a, b=b);
    DMux(in=c2d, sel=sel[0], a=c, b=d);
    DMux(in=e2f, sel=sel[0], a=e, b=f);
    DMux(in=g2h, sel=sel[0], a=g, b=h);
}

```