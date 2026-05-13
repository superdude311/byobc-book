# L5 - Assembly Programming 1

[Lecture slides link](https://docs.google.com/presentation/d/1MCiXMiFU_-juLWtCAMQpGVCYo8OorCdi07UXzMjd7CA/edit?usp=sharing)

## Lecture

With storage and memory attached to our computer,
we're finally ready to write some programs.

### Machine Code and Assembly

We've already seen glimpses of how
CPU instructions are represented as bytes.
For example, we explored the behavior of the instruction `$EA`
in chapter 2.

It shouldn't come as a surprise that different bytes
produce different behaviors.
If we were determined enough,
we could look up a table of every possible instruction byte on the 6502,
and write an entire program with hand-picked bytes.
This is known as writing **machine code**,
and it is the very lowest layer of programming abstraction possible on most processors.

However, writing raw machine code is both tedious and difficult.
It would be much easier if, instead of remembering the byte values,
we could just write down an easy-to-remember name for each instruction.
This is the principle behind an **assembly language**:
we write programs in a simple text-based programming language,
which then is easy to translate back into machine code.

Once we write our program in an assembly language,
we use a program called an **assembler**
to translate the assembly back into machine code that can be directly run.

### Anatomy of an Instruction

If we take the `$EA` machine-code instruction as an example,
you may recall that we've already given it a name: `NOP`, short for "no operation."
This name is known as a **mnemonic**.

There are a total of 60 distinct instructions on the 65C02,
each of which has its own mnemonic.
Each of those instructions has a byte that encodes the instruction,
and this byte is known as the instruction's **opcode**.

Some instructions might also have one or two additional bytes after the opcode,
and these additional bytes are known as the **operand**.

Most instructions are more complicated than `NOP`, though.
Lets learn more about what these instructions are actually doing.

### Registers!

Most instructions change the **registers**, which are small, quickly accessable units of storage inside the CPU.
All reasonable CPUs have registers. The 6502 is rather rudimentary, and thus only has 3 registers.
These are the A, X, and Y registers. The A register is the **accumulator**, 
and the X and Y registers are index registers. Each register holds a 1-byte value.

### Loading from Registers

To load a value into a register, use the `LDA`, `LDX`, and `LDY` instructions.
There are two main ways to tell the CPU where to load the value from. These are called **addressing modes**.
The first is **Immediate Addressing Mode**, which loads an "immediate" value. This is denoted with a `#`.
For example, the operation `LDA #42` will load the value `$42` to the A register.
The other is **Implicit? Addressing Mode**, which loads the value at that address in memory to the register. This is not denoted by a special character.
For example, the operation `LDA 1234` will load A with the value which is at the memory address `$1234`.

### Storing to Registers

Often we also want to store values from our registers into memory. 
This can be accomplished with a few more instructions, namely `STA`, `STX`, and `STY`.
For example, to store the value held into the A register to memory address `$1234`, we can use `STA $1234`.

### Increment and Decrement

The instructions `INX/DEX`, `INY/DEY`, and `INC A/DEC A`, are used for incrementing and decrementing X, Y, and A respectively.

### Jumps

The `JMP` immediately goes to another instruction, by setting the Program Counter to another memory address.
For example, if the PC is currently `$8004`, `JMP $8004` will skip the instructions at `$8001-8003`.
However, we don't like remembering arbitrary addresses, so we use labels for readibility. Labels are denoted as the following
```
    JMP some_label  ; will jump to LDX instruction
    NOP
    NOP
some_label:
    LDX #42
```
Jumps can also go forward and backwards, and are helpful for implementing loops. 
Let's look at the following loop in C before translating it into assembly.
```C
int x = 10;
do {
    x = x - 1
} while (true);
```
In 6502 assembly, this could be written as
```
    LDX #10
label:
    DEX
    JMP label
```
However, an infinite loop is not very useful. 
We'd like to be able jump only on certain conditions, like `x < 10` or `a != 42`.
We need some kind of... conditional jump

### Conditinal Jumps



### Doing Math on the Computer
TODO:

## Assignment

There's no hardware changes this time!
Instead, you're going to fill in a program.

Make sure that you've run one of the `install_windows`, `install_macos`, or `install_linux` scripts,
as this will install DASM for you.

Navigate to the directory where you initially cloned the `byobc` repository, 
then open up `starter-code/sum_1_to_N.S`.
This program starts zeroing out the A register
and loading some number into the X register (in this case, 10).

Write code beneath `sum_loop` according to the comments,
and then deploy this program to your board.
Check previous chapters for the command if you don't remember.

Use the debugger to run your program.
If it worked, then once you get to `halt`,
the A register should contain the sum of the numbers from 1 to 10
(which will be displayed *in hexadecimal* as `37`).
