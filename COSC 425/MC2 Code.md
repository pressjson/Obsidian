Just a bunch of examples of MC2 from Computer Systems, 5e

The PEP/9 CPU:

![[Pasted image 20250414091239.png]]

This is MC2, but it's in C++ to have syntax highlighting

## Problems
### Problem 12.29(a)

```c++
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

