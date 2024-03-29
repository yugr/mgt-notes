Things to consider for inclusion into compiler contract
(or at least discussing them with the contractor).

Criteria for selecting a contractor:
* Prior experience in LLVM (target ports, large compiler features)
* Example works
* Existing open-source commits

# Functional requirements

Frontend:
  * Languages (C, C++, C++11, OpenCL, OpenMP, OpenACC)
  * Language features:
    * C++ exceptions
    * builtin/OpenMP pragmas
    * dynamic alloca
    * atomics
    * setjmp/longjmp
    * inline asm
    * floating point
    * long long
  * Integrated asm/disasm
  * Vector support
    * types (including mask types)
    * intrinsics (which part of ISA is covered e.g. how masking will be supported)

Middlend:
  * target-independent optimizations:
    * autovec (which part of ISA is covered)
    * LTO
    * PGO
    * vectorization (including predication)
    * if-conversion
    * load/store aggregation
    * SWP
    * loop unrolling
  * debugging features
    * sanitizers
    * CFI

Backend:
  * target-dependent optimizations:
    * support of ISel of specific parts of ISA (MACs, vector operators, etc.)
    * predication (just cond. moves or arbitrary instructions)
    * bundling and scheduling
      * how instruction latencies will be modelled? (ideally autogenerated from specs)
    * postincrement/postdecrement instructions
    * delay slots
    * HW loops
    * other target-specific optimizations
  * supported ISA variants
  * codegeneration options:
    * PIC
    * overlays
    * linker scripts
    * uninitialized segments

Runtime libs:
  * compiler-rt
  * libc++
  * parallel runtimes (OpenMP, OpenACC, OpenCL)

Tools:
  * Binutils replacements
  * llvm-mca
  * static analyzer

Documentation:
  * howto guides (build, test)
  * open-source changes (new passes, changed defaults, etc.)

External tools:
  * integration with debugger (call stacks, variable printing, etc.)
  * integration with existing libs (libc, STL, BSP libs, etc.)
  * ABI compatibility

# Quality requirements

* Base version of LLVM (ideally the latest release)
* OSS history must be preserved in new repo (to simplify future work)
* Allowed modifications of OSS code
  * all modifications need to be marked with ifdef-directives
  * all large enough (e.g. > 20 LOC) modifications need to be pre-approved
* Static (`-Werror`, Prevent, etc.) and dynamic (sanitizers, Valgrind, etc.) analyses
* Testsuites
  * LNT, GCC suite, existing company codes, SPEC, etc.
    (versions and links must be specified explicitly)
  * all test modifications/removals need to be pre-approved
    * benchmark modifications are extremely undesirable as they complicate platform comparisons in future
  * pass rates e.g. 99% (existing tests must pass 100%)
  * testing must be performed on customer's servers
  * test flags (-O0/O1/O2/O3/Og, autovec, -ffast-math, `-verify-machineintrs`, etc.)
  * tested compiler must be built with `-DLLVM_ENABLE_ASSERTIONS`
  * host platforms (Windows, Linux)
  * target platforms (simulators, HW versions)
* Performance requirements
  * benchmark list (SPEC, CoreMark, EEMBC, etc.)
  * performance must not be lower than existing compiler, if any

# Collaboration requirements

* contractor's SLA - time bounds for servicing common customer requests:
  * answer questions
  * triage/fix bugreports (at least for critical bugs)
    * for bugs found during development
    * for bugs found after project ends
  * review/merge customer's patches
  * update components provided by customer (e.g. new simulators)
  * definition of severity
  (this is an important item because handling requests detains contractor
  from his main work)
* customer's SLA - time bounds for servicing contractor's requests
  * which components customer is responsible for (simulators, Binutils, target-specific libs, etc.)
* tracking work in Jira:
  * all work must be tracked in Jira
  * all tasks must contain proper description and acceptance criteria
  * work hours must be tracked
  * all tasks must be assigned to corresponding Epics/Components
  * all technical discussions must happen in tracker
  * allocate several Jira accounts for customers employees
* configuration management:
  * all merges to master are done through GH/GL/Bitbucket pull requests
  * no complex history (`git pull --rebase --no-ff` for all feature branches)
* all significant compiler changes must be pre-approved by the customer:
  * open-source modifications
  * disabling standard OSS passes
  * adding custom passes
* milestones
  * proposed stages:
    * ???
  * ideally helloworld should compile and run successfully ASAP
    (as this will allow customer to start experiments)
* reports
  * weekly report (what was achieved, plan for next week, blockers)
