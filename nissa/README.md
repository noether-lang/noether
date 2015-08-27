Nissa
=====

Nissa is:

* An intermediate representation --Nissa IR-- for compilers
  and interpreters, based on Static Single Assignment form;
* A library, written in [Idris](http://www.idris-lang.org/),
  to manipulate and optimize Nissa IR programs;
* A run-time system providing a garbage collector, a
  work-stealing scheduler, and other essential infrastructure
  for programs compiled to or via Nissa IR. This will also
  include a Nissa IR interpreter.

Nissa is currently in an early development stage.

Low-level code generation and optimization will be supported
via an interface to [LLVM](http://llvm.org/) backends, at
least initially. The main focus of Nissa is on higher-level
optimizations and transformations needed to implement particular
programming languages, such as Noether. A longer-term goal is
to support fully verified compilation down to code generation
for specific targets.

Nissa is [BSD-licensed](../LICENSE), like Idris and the rest
of the Noether implementation.

Why a new IR? Why not just use LLVM, Firm, MLRISC, etc.?

* This library is written in Idris, which is a memory-safe,
  functional, dependently-typed language. LLVM is written
  in C++ and Firm in C, which are not memory-safe. MLRISC
  is written in Standard ML which is not dependently typed.

* Nissa will be designed specifically to support verified,
  type-preserving compilation. The Idris library uses
  dependent types for the in-memory IR to ensure that
  invariants are proven to hold and that type safety is
  preserved. Its APIs are designed to be used functionally,
  without any requirement for in-place modification.

* Nissa is well-suited to implementing capability-secure
  languages like Noether. There is no implicit "ambient"
  authority or mutable state at the IR level, instead
  access to these must be granted via capabilities
  that are passed explicitly. The IR is also completely
  memory-safe, given certain explicit assumptions about the
  run-time system.

* There is direct support for write barriers needed for
  concurrent garbage collection and "recovery block"-style
  transactional rollback. The IR allows write barrier elision
  and hoisting optimizations to be expressed safely. (Note:
  this support may be somewhat specialized to the needs of
  Noether.)

* Nissa IR is also designed to be efficiently interpretable.
  More precisely, a "loader" can perform a lightweight
  validation and transformation of any valid IR program into
  a form suited to interpretation. Because the IR is typesafe
  and memory-safe, it could be used for mobile code
  applications in a similar way to JVM byte code.
  The interpreter also provides a reference implementation that
  does not depend on LLVM, allowing it to be fully verified.