# Přehled předmětů a otázek

- algoritmy: PAL, TAL, KO, PAG
- computer: PAP, ESW
- embedded & hardware: ISC, AVS, KRP

## PAL - [poznámky](/PAL.md)

- **Formality:** Notation of the asymptotic complexity of an algorithm. Basic notation of graph problems - degree, path, circuit, cycle. Graph representations by adjacency, distance, Laplacian, and incidence matrices. Adjacency list representation.
    
- **MST:** Algorithms for a minimum spanning tree (Prim-Jarnik, Kruskal, Boruvka), strongly connected components (Kosarju-Sharir, Tarjan), and Euler trail. Uninon-find problem. Graph isomorphism, tree isomorphism.
    
- **Enumeration & Combination:** Generation and enumeration of combinatorial objects - subsets, k-element subset, permutations. Gray codes. Prime numbers, sieve of Eratosthenes. Pseudorandom numbers properties. Linear congruential generator.

- **Trees:** Search trees - data structures, operations, and their complexities. Binary tree, AVL tree, red-black tree (RB-tree), B-tree and B+ tree, splay tree, k-d tree. Nearest neighbor searching in k-d trees. Skip list.
- Finite automata, regular expressions, operations over regular languages. Bit representation of nondeterministic finite automata. Text search algorithms - exact pattern matching, approximate pattern matching (Hamming and Levenshtein distance), dictionary automata.

## TAL

- Asymptotic growth of functions, time and space complexity of algorithms. Correctness of algorithms - variant and invariant.
- Deterministic Turing machines, multitape Turing machines, and Nondeterministic Turing machines.
- Decision problems and languages. Complexity classes P, NP, co-NP. Reduction and polynomial reduction, class NPC. Cook theorem. Heuristics and approximate algorithms for solving NP-complete problems.
- Classes based on space complexity: PSPACE and NPSCAPECE. Savitch Theorem.
- Randomized algorithms. Randomized Turing machines. Classes based on randomization: RP, ZPP, co-RP.
- Decidability and undecidability. Recursive and recursive enumerable languages. Diagonal language. Universal language and Universal Truing machine.

## KO

- **Integer Linear Programming:** Shortes paths problem and traveling salesman problem ILP formulations. Branch and Bound algorithm. Problem formulations using ILP. Special ILP problems solvable in polynomial time.
- **Shortest paths problem:** Dijkstra, Bellman-Ford, and Floyd-Warshall algorithms. Shortest paths in directed acyclic graphs. Problem formulations using shortest paths.
- **Network flows:** Maximum flow and minimum cut problems. Ford-Fulkerson algorithm. Feasible flow with balances. Minimum cost flow and cycle-canceling algorithm. Problem formulations using network flows. Maximum cardinality matching.
- **Knapsack problem:** Approximation algorithm, dynamic programming approach, approximation scheme.
- **Traveling salesman problem (TSP):** Double-tree algorithm and Cristofides algorithm for the metric problem. Local search k-OPT.
- **Scheduling:** problem description and notation. One resource - Bratley algorithm, Horn algorithm. Parallel identical resources - list scheduling, dynamic programming. Project scheduling with temporal constraints - relative order and time-indexed ILP formulations.
- **Constrant Safisfaction Problem (CSP):** AC3 algorithm.

## ISC

- Main features and economical aspects of the Application specific integrated circuits systems: full custom design, gate array, standard cells, programmable array logic.
- Design principles of mix-signal integrated circuits, purpose of hierarchical design, digital and analogue block interface, CAD design tolls for automatic circuit generations, functional and static time analysis, formal verification, Verilog-A, Verilog-AMS, VHDL-A.
- **Front-end design:** functional specification, RTL, logic synthesis, Gate-level netlist, behavioral stimulus extraction.
- **Back-end design:** specification of Design Kit, Floorplanning, place, and route, layout, parasitic extraction, layout versus schema check (LVS).
- Tape out and IC fabrication process, integrated systems verification, scaling and design mapping to different technologies.

## PAP

- Superscalar techniques used in nodes of multiprocessor systems, data flow inside the processor, Tomasulo algorithm and its deficiencies, pracise exceptions support, architectural state, register renaming, reservation station, reorder buffer, instruction fetch, decode, disptath, issue, execute, finish, complete, reorder, branch prediction, store forwarding, hit under miss.
- Relation between memory coherency and consistency their implementation on systems with shared bus and when multiple rings topologies are used, MESI, MOESI, home directory.
- Rules for execution synchronization and data exchange in multiprocessor systems, mute implementation, relation to consistency models and mechanism to achieve expected algorithms behavior on systems with relaxed consistency models (PRAM, PSO, TSO, PC, barrier instructions).
- SMP and NUMA nodes interconnections networks, conflicts and rearrangeable networks Beneš network.
- Parallel computations on multiprocessor systems, OpenMP on NUMA and MPI on distributed memory systems, their combinations.

## KRP

- USB I/O subsystem, structure and functionality of elelemtns, protocol stack, transfer - transaction - packet hierarchy, transfer types and pipes, bandwidth allocation principles enumartion process and PnP, descriptor hierarchy, ISB device implementation.
- PCI Express (CPI) I/O subsystems, basic differences and commons of PCI and PCIe, protocol stack, transaction types, packet routing principles, quality of service support, PnP and enumeration process.
- Ethernet based networking, VLAN, precision time protocol (PTP), stream reservation protocol (SRP), time sensitive networks (TSN).
- In-vehicle networking, Controller Area Network (CAN, CAN-FD), Local Interconnect Network (LIN), FlexRay, data-link layer algorithms, physical topology constraints and relation to system design.

## AVS

- Typical architecture and main feature of ARM based microcontrollers. AMBA. I/O pin configuration. Common used peripheral circuits (I/O ports, timers, DMA controllers, NVIC controller, JTAG, SWD, A/D converters, D/A converters, SPI controllers, I2C controllers, UART, FLASH and SRAM memory).
- Typical architecture and main features of digital signal processors (DSP). Common used peripheral circuits. Special computational units and their features (ALU, MAC, SHIFT BARREL, register, DAG).
- Digital signal processing: signal spectrum analysis (DFT, IDFT), correlation functions and their typical use, digital filters (FIR, IIR), signal interpolation, signal decimation.
- Types of A/D converters. Sampling theorem. Anti-aliasing filter (AAF). Direct digital synthesis (DDS).
- Used controls interfacing to microcontrollers (buttons, rotary encoders, graphic LCD, audio codecs, power switches, relays, contactors). Motion control (brush DC motor, stepper motor and brushless DC motor control).

## PAG

- Describle basic communication operations used in parallel algorithm. Show const analysis of one-to-all broadcast, all-to-all broadcast, scatter, and all-to-all personalized communication on a ring, mesh, and hypercube. Describe All-Reduce and Prefix-Sum operations and outline their usage.
- Describe performance metrics and scalability for parallel systems. How efficiency of a parallel algorithm depends on the problem size and the number of processors? Derive isoefficiency functions of a parallel algorithm for adding numbers(including communication between processors) and explain how it characterizes the algorithm.
- Explain and compare two parallel algorithms for matrix-vector multiplication. Describe a parallel algorithm for matrix-matrix multiplication and explain the idea of Cannon’s algorithm. Discuss the principle and properties of the DNS algorithm used for matrix-matrix multiplication.
- Outline the principle of sorting networks and describe parallel bitonic sort, including its scalability. Explain parallel enumeration sort algorithm on PRAM model, including its scalability.
- Explain all steps of a parallel algorithm for finding connected components in a graph given by the adjacency matrix. Using an example, illustrate a parallel algorithm for finding a maximal independent set in a sparse graph.

## ESW

- Java Virtual Machine, memory layout, frame, stack-orimented machine processing, ordinary object pointer, compressed ordinary object pointer. JVM bytecode, Just-in-time compiler, tired compilation, on-stack replacement, disassembler, decompiler. Global and local safe point, time to safe point. Automatic memory Management, generational hypothesis, garbage collectors. CPU and memory profiling, sampling and tracing approach, warm-up phase.
- Dataraces, CPU pipelining and superscalar architecture, memory barrier, volatile variable. Synchronization- thin, fat and biased locking, reentrant locks. Atomic operations based on compare-and-set instructions, atomic field updaters. Non-blocking algorithms, wait free algorithms, non-blocking stack (LIFO).
- Static and dynamic memory analysis, shallow and retained size, memory leak. Data Structures, Java primitives and objects, auto-boxing and unboxing, memory efficiencyofcomplex data structures. Collection for performance, type specific collections, open addressing hashing, collision resolution schemes. Bloom filters, complexity, false positives, bloom filter extensions. Reference types- weak, soft, phantom.
- JVMobject allocation, thread-local allocation buffers, object escape analysis, data locality, non-uniform memory allocation.
- Networking, OSI model, C10K problem. Blocking and non-blocking input/output, threading server, event-driven server. Event-based input/output approaches. Native buffers in JVM, channels and selectors.
- Synchronizationinmulti-threadedprograms(atomicoperations,mutex,semaphore,rw-lock, spinlock, RCU). When to use which mechanism? Performance bottlenecks of the mentioned mechanisms. Synchronization in “read-mostly workloads”, advantages and disadvantages of different synchronization mechanisms.
- Cache-efficient data structures and algorithms (e.g., matrix multiplication). Principles of cache memories, different kinds of cache misses. Self-evicting code, false sharing– what is it and howdeal with it?
- Profiling and optimizations of programs in compiled languages (e.g., C/C++). Hardware performance counters, profile-guided optimization. Basics of C/C++ compilers, AST, intermediate representation, high-level and low-level optimization passes.