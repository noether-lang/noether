Nissa
=====

Nissa is:

* An intermediate representation --Nissa IR-- for compilers
  and interpreters, based on [Static Single Assignment form]
  (https://en.wikipedia.org/wiki/Static_single_assignment_form);
* A library, written in [Idris](http://www.idris-lang.org/),
  to manipulate and optimize Nissa IR programs;
* A run-time system providing a garbage collector, a
  work-stealing scheduler, and other essential infrastructure
  for programs compiled to or via Nissa IR. This will also
  include a Nissa IR interpreter.

Nissa is currently in a very early development stage.

The main focus of Nissa is on higher-level optimizations and
transformations needed to implement particular programming
languages, such as Noether. Initially, low-level code generation
and optimization will be supported via an interface to
[LLVM](http://llvm.org/); in the longer term, the intention
is to support fully verified compilation including code
generation for specific targets.

Nissa is [BSD-licensed](../LICENSE), like Idris and the rest
of the Noether implementation.


Why a new IR? Why not just use LLVM, Firm, MLRISC, etc.?

* This library is written in Idris, which is a memory-safe,
  functional, dependently typed language. LLVM is written
  in C++ and [Firm](http://pp.ipd.kit.edu/firm/) in C,
  which are not memory-safe.
  [MLRISC](http://www.cs.nyu.edu/leunga/www/MLRISC/Doc/html/INTRO.html)
  is written in Standard ML which is not dependently typed.

* Nissa will be designed specifically to support verified,
  type-preserving compilation. The Idris library will use
  dependent types for the in-memory IR to ensure that
  invariants are proven to hold and that type safety is
  preserved. Its APIs will be designed to be used functionally,
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
  More precisely, a loader can perform a lightweight
  validation and transformation of any valid IR program into
  a form suited to interpretation. Because the IR is typesafe
  and memory-safe, it could be used for mobile code
  applications in a similar way to JVM byte code.
  The interpreter also provides a reference implementation that
  does not depend on LLVM, allowing it to be fully verified.


Why not use CompCert or Vellvm?

* The [license for CompCert](https://github.com/AbsInt/CompCert/blob/master/LICENSE)
  is not open-source -- it does not allow commercial use.
  [Vellvm](https://www.cis.upenn.edu/~stevez/vellvm/) uses
  CompCert libraries and therefore is also not open source.

* I prefer the approach to dependent typing taken by
  Idris to that of Coq, and believe it will be more
  conducive to writing the implementations of Noether and
  Nissa.


What does "Nissa" mean?

* It can be expanded as "Noether Interpretable Static Single Assignment".
  Nissa is also a name with [various meanings](http://www.babynamespedia.com/meaning/Nissa).
