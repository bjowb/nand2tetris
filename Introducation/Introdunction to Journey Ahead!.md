
### Introduction

- every one of these hardware devices is constructed from lower-level, elementary logic gates. 
- And these gates, in turn, can be built from primitive gates like #nand and #nor

### Nand to tetris

- First, all computers are based, at bottom, on elementary logic gates, of which #nand is the most widely used in industry
- Second, every general-purpose computer can be programmed to run a Tetris game, as well as any other program that tickles your fancy.
- Starting at the bottom tier of the figure, any general-purpose computer has an architecture that includes a CPU (processor) and a RAM (memory).
- All CPU and RAM devices are made of elementary logic gates. And, surprisingly and fortunately, as we will soon see, all logic gates can be made from Nand gates alone.
- Focusing on the software hierarchy, all high- level languages rely on a suite of translators (compiler/interpreter, virtual machine, assembler) for reducing high-level code all the way down to machine-level instructions.
- Some high-level languages are interpreted rather than compiled, and some don't use a virtual machine, but the big picture is essentially the same. 
- This observation is a manifestation of a fundamental computer science principle, known as the Church-Turing conjecture: at bottom, all computers are essentially equivalent.


![[Pasted image 20260222163911.png]]

- Computer systems are described top-down, showing how high-level abstractions can be reduced to, or realized by, simpler ones. For example, we can describe how binary machine instructions executing on the computer architecture are broken into micro-codes that travel through the architecture's wires and end up manipulating the lower-level ALU and RAM chips.
- Alternatively, we can go bottom-up, describing how the ALU and RAM chips are judiciously designed to execute micro-codes that, taken together, form binary machine instructions. Both the top-down and the bottom-up approaches are enlightening.

### Methodology
- We will use HDL.
- The hardware platform is based on a set of about 30 logic gates and chips, built in Part I of the book. 
- Every one of these gates and chips, including the topmost computer architecture, will be built using a Hardware Description Language.

