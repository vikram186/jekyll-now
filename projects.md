---
layout: page
title: Projects
---

### Compiler-assisted randomization using LLVM
In this project we present a novel *compiler-assisted approach* for protecting a program against *return-oriented programming (ROP)* attacks. During compile time we increase the code size of each function of the program by adding no-op instructions after each function epilogue. By modifying the binary loader of the operating system we can then randomly shift the instructions inside the function bodies, thereby completely randomizing the binary without breaking the semantics of the program. Since we leave the function addresses unchanged for simplicity reasons, we also propose a simplistic mechanism to protect against entry-point gadgets, which would otherwise still be valid attack vectors. With those two approaches in tandem, we significantly decrease the chances of an attacker successfully crafting a working gadget chain.

A prototype implemetation is here [gadgetSmash](https://github.com/arkivm/gadgetsmash).


### Lightweight Capability Domains (LCDs)
LCDs build on the insight that, lack of isolation between kernel subsystems pose a serious threat to the security of the system. In commodity kernels like Linux, Windows and Mac, isolation was eschewed for performance reasons and lack of support in processor architectures. LCDs introduce a microkernel paradigm into a monolithic Linux kernel for isolating subsystems. LCDs run subsystems of a commodity OS, e.g., device drivers, file systems, network stacks, inside strongly isolated containers. The isolated code, however, needs to communicate with the rest of the kernel. We have introduced an Interface Description Language (IDL) to provide a transparent way of communicating across isolated subsystems and synchronizing their state.

More information here [LCD PLOS15](http://www.ics.uci.edu/~aburtsev/doc/plos15-lcds.pdf)

### Extracting struct field names from LLVMDebugInfo
For interprocedural analysis, one needs the field names of datastructures (e.g., struct) inside the kernel. This is just a minimal implementation on how to do it with LLVMDebugInfo. I have implemented an LLVM pass on top of Adrian's skeleton pass. When run, it prints out the structure and corresponding field names.

A prototype implemetation is here [structfieldnames](https://gitlab.flux.utah.edu/deker/llvm_pass_structfields/tree/release_39).
