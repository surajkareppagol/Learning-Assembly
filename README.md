# Learning Assembly

> `Quick Note`: This content is from [Tutorials Planet](https://www.tutorialspoint.com/).

Assembly programs are composed of `data`, `bss` and `text` sections.

- `data` section
  Used for initialized data or constants, and data does not change at runtime.

```asm
section.data
```

- `bss` section
  Used for declaring variables.

```asm
section.bss
```

- `text` section
  Used for keeping actual code, begins with declaration `global_start`, which is the start point of the program.

```asm
section.text
  global _start

_start:
```

## Some More Components

- `Comments`
  Comments begin with `;`.

There are three types of statements.

- Executable instructions or instructions
- Assembler directives or pseudo-ops
- Macros

## Syntax

Statements are entered one line at a time. The fields in the square bracket are optional.

```asm
[label] mnemonic [operands] [;comments]
```

```asm
section.text
    global _start

_start:
    mov edx, len
    mov ecx, msg
    mov ebx, 1
    mov eax, 4
    int 0x80

    mov eax, 1
    int 0x80

section.data
msg db  "Hello World", 0xa
len equ $ - msg
```

To execute,

> Note: Here spaces are used between words, so please take care of that.

```bash
nasm -f elf Hello\ World.asm
ld -m elf_i386 -s -o Hello\ World Hello\ World.o
```

Here `section` and be changed to `segment`.

## Memory Segments

A segmented memory model divides the system memory into groups of independent segments referenced by pointers located in the segment registers.

Each segment is used to contain a specific type of data. One segment is used to contain instruction codes, another segment stores the data elements, and a third segment keeps the program stack.

- `Data Segment`
  It is represented by .data section and the .bss. The .data section is used to declare the memory region, where data elements are stored for the program. This section cannot be expanded after the data elements are declared, and it remains static throughout the program. `.bss section` is also a static memory section that contains buffers for data to be declared later in the program. This buffer memory is zero-filled.

- `Code Segment`
  It is represented by .text section. This defines an area in memory that stores the instruction codes. This is also a fixed area.

- `Stack`
  This segment contains data values passed to functions and procedures within the program.

## Registers

There are `ten 32-bit` and `six 16-bit` processor registers in IA-32 architecture. The registers are grouped into three categories.

- General registers
- Control registers
- Segment registers

The general registers are further divided into the following groups.

- Data registers
- Pointer registers
- Index registers

### Data Registers

Four 32-bit data registers are used, these are -

- Complete 32-bit data registers: `EAX`, `EBX`, `ECX` and `EDX`.
- Lower halves of the 32-bit registers can be used as four 16-bit data registers: `AX`, `BX`, `CX` and `DX`.
- Lower and higher halves of the above-mentioned four 16-bit registers can be used as eight 8-bit data registers: `AH`, `AL`, `BH`, `BL`, `CH`, `CL`, `DH`, and `DL`.

![Data Registers - Image from TutorialsPoint](https://www.tutorialspoint.com/assembly_programming/images/register1.jpg)

- `AX` is the primary `accumulator`; it is used in input/output and most arithmetic instructions. For example, in multiplication operation, one operand is stored in EAX or AX or AL register according to the size of the operand.

- `BX` is known as the `base` register, as it could be used in indexed addressing.

- `CX` is known as the `count` register, as the ECX, CX registers store the loop count in iterative operations.

- `DX` is known as the `data` register. It is also used in input/output operations. It is also used with AX register along with DX for multiply and divide operations involving large values.

### Pointer Registers

The pointer registers are 32-bit EIP, ESP, and EBP registers and corresponding 16-bit right portions IP, SP, and BP. There are three categories of pointer registers −

![Pointer Registers - Image from TutorialsPoint](https://www.tutorialspoint.com/assembly_programming/images/register3.jpg)

- `Instruction Pointer (IP)` − The 16-bit IP register stores the offset address of the next instruction to be executed. IP in association with the CS register (as CS:IP) gives the complete address of the current instruction in the code segment.

- `Stack Pointer (SP)` − The 16-bit SP register provides the offset value within the program stack. SP in association with the SS register (SS:SP) refers to be current position of data or address within the program stack.

- `Base Pointer (BP)` − The 16-bit BP register mainly helps in referencing the parameter variables passed to a subroutine. The address in SS register is combined with the offset in BP to get the location of the parameter. BP can also be combined with DI and SI as base register for special addressing.

### Index Registers

The 32-bit index registers, ESI and EDI, and their 16-bit rightmost portions. SI and DI, are used for indexed addressing and sometimes used in addition and subtraction. There are two sets of index pointers −

- `Source Index (SI)` − It is used as source index for string operations.

- `Destination Index (DI)` − It is used as destination index for string operations.

![Index Registers - Image from TutorialsPoint](https://www.tutorialspoint.com/assembly_programming/images/register2.jpg)

### Control Registers

The 32-bit instruction pointer register and the 32-bit flags register combined are considered as the control registers.

Many instructions involve comparisons and mathematical calculations and change the status of the flags and some other conditional instructions test the value of these status flags to take the control flow to other location.

The common flag bits are:

- `Overflow Flag (OF)` − It indicates the overflow of a high-order bit (leftmost bit) of data after a signed arithmetic operation.

- `Direction Flag (DF)` − It determines left or right direction for moving or comparing string data. When the DF value is 0, the string operation takes left-to-right direction and when the value is set to 1, the string operation takes right-to-left direction.

- `Interrupt Flag (IF)` − It determines whether the external interrupts like keyboard entry, etc., are to be ignored or processed. It disables the external interrupt when the value is 0 and enables interrupts when set to 1.

- `Trap Flag (TF)` − It allows setting the operation of the processor in single-step mode. The DEBUG program we used sets the trap flag, so we could step through the execution one instruction at a time.

- `Sign Flag (SF)` − It shows the sign of the result of an arithmetic operation. This flag is set according to the sign of a data item following the arithmetic operation. The sign is indicated by the high-order of leftmost bit. A positive result clears the value of SF to 0 and negative result sets it to 1.

- `Zero Flag (ZF)` − It indicates the result of an arithmetic or comparison operation. A nonzero result clears the zero flag to 0, and a zero result sets it to 1.

- `Auxiliary Carry Flag (AF)` − It contains the carry from bit 3 to bit 4 following an arithmetic operation; used for specialized arithmetic. The AF is set when a 1-byte arithmetic operation causes a carry from bit 3 into bit 4.

- `Parity Flag (PF)` − It indicates the total number of 1-bits in the result obtained from an arithmetic operation. An even number of 1-bits clears the parity flag to 0 and an odd number of 1-bits sets the parity flag to 1.

- `Carry Flag (CF)` − It contains the carry of 0 or 1 from a high-order bit (leftmost) after an arithmetic operation. It also stores the contents of last bit of a shift or rotate operation.
