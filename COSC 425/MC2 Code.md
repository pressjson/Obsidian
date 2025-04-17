Just a bunch of examples of MC2 from Computer Systems, 5e

The PEP/9 CPU:

![[Pasted image 20250414091239.png]]

This is MC2, but it's in C++ to have syntax highlighting

## Problems
### Problem 12.29(a)

```cpp++
// Problem 12.29(a)
// MOVSPA
// RTL: A <- SP  

UnitPre: IR=0x030000, SP=0xABCD
UnitPost: A=0xABCD

// Send lower byte
A=5, AMux=1, ALU=0, CMux=1, C=1; LoadCk
// Send higher byte
A=4, AMux=1, ALU=0, CMux=1, C=0; LoadCk
```

### Figure 12.11

When adding, check the Z bit in *both* the low and the high order bits. 

> - If the AndZ control signal is 0, Zout passes directly through to the output.
> - If the AndZ control signal is 1, Zout AND Z passes to the output.

```c++
// Figure 12.11
// ADDA this,i
// RTL: A <- A + Oprnd; N <- A<0, Z <- A=0, V <- {overflow}, C <- {carry}
// Immediate addressing: Oprnd = OprndSpec
  
UnitPre: IR=0x700FF0, A=0x0F11, N=1, Z=1, V=1, C=1, S=0
UnitPost: A=0x1F01, N=0, Z=0, V=0, C=0

// add lower two bits
A=1, B=10, AMux=1, ALU=1, CMux=1, AndZ=0, C=1; LoadCk, SCk, ZCk
// add higher two bits
A=0, B=9, AMux=1, CSMux=1, ALU=2, AndZ=1, CMux=1, C=0; LoadCk, NCk, ZCk, Vck, CCk
```

### Figure 12.12

Since this is so complex, Prof. Cupp says that this will not be on the exam, but it's still good to go over.

```cpp
// Figure 12.12
// LDWX this,n
// RTL: X <- Oprnd; N <- X<0, Z <- X=0
// Indirect addressing: Oprnd = Mem[Mem[OprndSpec]]
 
UnitPre: IR=0xCA0012, Mem[0x0012]=0x26D1, Mem[0x26D1]=0x53AC
UnitPre: N=1, Z=1, V=0, C=1, S=1
UnitPost: X=0x53AC, N=0, Z=0, V=0, C=1

// T3<high> <- Mem[OprndSpec], T2 <- OprndSpec + 1.
A=9, B=10; MARCk
MemRead, A=10, B=23, AMux=1, ALU=1, CMux=1, C=13; SCk, LoadCk
MemRead, A=9, B=22, AMux=1, CSMux=1, ALU=2, CMux=1, C=12; LoadCk
MemRead, MDRMux=0; MDRCk
A=12, B=13, AMux=0, ALU=0, CMux=1, C=14; MARCk, LoadCk

// T3<low> <- Mem[T2].
MemRead
MemRead
MemRead, MDRMux=0; MDRCk
AMux=0, ALU=0, CMux=1, C=15; LoadCk

// Assert: T3 contains the address of the operand.
// X<high> <- Mem[T3], T4 <- T3 + 1.
A=14, B=15; MARCk
MemRead, A=15, B=23, AMux=1, ALU=1, CMux=1, C=17; SCk, LoadCk
MemRead, A=14, B=22, AMux=1, CSMux=1, ALU=2, CMux=1, C=16; LoadCk
MemRead, MDRMux=0; MDRCk
A=16, B=17, AMux=0, ALU=0, AndZ=0, CMux=1, C=2; NCk, ZCk, MARCk, LoadCk

// X<low> <- Mem[T4].
MemRead
MemRead
MemRead, MDRMux=0; MDRCk
AMux=0, ALU=0, AndZ=1, CMux=1, C=3; ZCk, LoadCk
```