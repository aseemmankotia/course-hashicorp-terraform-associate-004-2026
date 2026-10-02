# Chapter 7 Lab: Writing HCL: Types, Loops, Validation & Sensitive Data

**Estimated time:** 25 minutes

## Exercise

Refactor a count-based resource block into a for_each using a map variable, add a variable validation block, mark an output as sensitive, and add a resource postcondition

## Key takeaway

Choose for_each over count for stable, unique-keyed resources, and remember sensitive marking hides CLI output only, not the state file itself.
