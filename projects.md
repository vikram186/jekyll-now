---
layout: page
title: Projects
---

### Redleaf

RedLeaf is a new operating system aimed at leveraging a safe, linear-typed programming language, Rust, for developing safe and provably secure systems. RedLeaf builds on two premises: (1) Rust's linear type system enables practical language safety even for systems with the tightest performance and resource budgets, e.g., OS kernels and firmware, (2) a combination of SMT-based reasoning and pointer discipline enforced by linear types provides a way to automate and simplify verification effort and scale it to the size of a small operating system kernel that can run firmware subsystems.

More information can be found on the project page [RedLeaf Home](https://mars-research.github.io/redleaf){:target="_blank"}

### Lightweight Virtualization Domains (LVDs)

Commodity operating systems execute core kernel subsystems in a single address space along with hundreds of dynamically loaded extensions and device drivers. Lack of isolation within the kernel implies that a vulnerability in any of the kernel subsystems or device drivers opens a way to mount a successful attack on the entire kernel.

Historically, isolation within the kernel remained prohibitive due to the high cost of hardware isolation primitives. Recent CPUs, however, bring a new set of mechanisms. Extended page-table (EPT) switching with VM functions and memory protection keys (MPKs) provide memory isolation and invocations across boundaries of protection domains with overheads comparable to system calls. Unfortunately, neither MPKs nor EPT switching provide architectural support for isolation of privileged ring 0 kernel code, i.e., control of privileged instructions and well-defined entry points to securely restore state of the system on transition between isolated domains.

LVDs develop a collection of techniques for lightweight isolation of privileged kernel code. To control execution of privileged instructions, we rely on a minimal hypervisor that transparently deprivileges the system into a non-root VT-x guest. We develop a new isolation boundary that leverages extended page table (EPT) switching with the VMFUNC instruction. We define a set of invariants that allows us to isolate kernel components in the face of an intricate execution model of the kernel, e.g., provide isolation of preemptable, concurrent interrupt handlers. To minimize overheads of virtualization, we develop support for exitless interrupt delivery across isolated domains.

More information can be found on the project page [LVDs Home](https://mars-research.github.io/lvds/){:target="_blank"}

### Lightweight Execution Domains (LXDs)

Modern operating systems are monolithic. Today, however,lack of isolation is one of the main factors undermining security of the kernel. Inherent complexity of the kernel code and rapid development pace combined with the use of unsafe,low-level programming language results in a steady streamof errors. Even after decades of efforts to make commodity kernels more secure, i.e., development of numerous static and dynamic approaches aimed to prevent exploitation of mostcommon errors, several hundreds of serious kernel vulnerabilities are reported every year. Unfortunately, in a monolithic kernel a single exploitable vulnerability potentially providesan attacker with access to the entire kernel.

Modern kernels need isolation as a practical means of confining the effects of exploits to individual kernel subsystems. Historically, introducing isolation in the kernel is hard. First, commodity hardware interfaces provide no support for efficient, fine-grained isolation. Second, the complexity of amodern kernel prevents a naive decomposition effort.

Lightweight Execution Domains (LXDs) takes astep towards enabling isolation in a full-featured operating system kernel. LXDs allow one to take an existing kernelsubsystem and run it inside an isolated domain with minimal or no modifications and with a minimal overhead. We evaluate our approach by developing isolated versions of several performance-critical device drivers in the Linux kernel.

More information can be found on the project page [LXDs Home](https://mars-research.github.io/lxds/){:target="_blank"}


### Compiler-assisted randomization using LLVM
In this project we present a novel *compiler-assisted approach* for protecting a program against *return-oriented programming (ROP)* attacks. During compile time we increase the code size of each function of the program by adding no-op instructions after each function epilogue. By modifying the binary loader of the operating system we can then randomly shift the instructions inside the function bodies, thereby completely randomizing the binary without breaking the semantics of the program. Since we leave the function addresses unchanged for simplicity reasons, we also propose a simplistic mechanism to protect against entry-point gadgets, which would otherwise still be valid attack vectors. With those two approaches in tandem, we significantly decrease the chances of an attacker successfully crafting a working gadget chain.

A prototype implemetation is here [gadgetSmash](https://github.com/arkivm/gadgetsmash){:target="_blank"}


### Extracting struct field names from LLVMDebugInfo
In an effort to automatically generate IDL files for LCDs project, we decided to use LLVM interprocedural datastructure analysis (DSA) on Linux kernel. To have access to the field names of datastructures (e.g., struct) inside the kernel, I have implemented an LLVM functionpass that analyzes arguments of every function and prints out the field names, if the argument is of struct pointer type. The base skeleton code is forked from Adrian's skeleton pass. This is just a minimal implementation on how to do it with LLVMDebugInfo.

A prototype implemetation is here [structfieldnames](https://gitlab.flux.utah.edu/deker/llvm_pass_structfields/tree/release_39){:target="_blank"}
