Just a bunch of examples of MC2 from Computer Systems, 5e

The PEP/9 CPU:

![[Pasted image 20250414091239.png]]

This is MC2, but it's in C++ to have syntax highlighting

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

