* Validate task description first (if available)
  * are all requirements clear?
  * are requirements complete? Is anything missing?
* Then verify the code:
  * does it implement _all_ task requirements?
  * are benchmark results provided?
  * are changes maintainable
    * probability of changes is minimized or cost of change will be low
    * are issues worked around or really fixed?
    * are issues fixed for all cases?
  * are there common issues? (ideally this should be done
    through a checklist)
    * formalitites: link to task tracker, changelog, etc.
    * codestyle violations
    * usage of patterns/constructs which are not widely used
      in existing codebase
    * code smells:
      * duplication
      * overly deep nesting
      * global variables
      * magic constants
      * goto
    * are all retcodes checked?
      * don't be paranoid - it's ok to not check every `malloc`
      * but not checking `fopen` is not...
    * are there enough comments?
    * have the docs been updated?
    * have new compiler warnings been introduced?
    * ABI violations in public APIs
    * project-specific rules
* Verify tests
  * how was the code tested? Is test coverage enough for reviewed change?
  * for optimizations, are changes in performance of important benchmarks
    beneficial/acceptable?
* In case of disagreement - contact PL
