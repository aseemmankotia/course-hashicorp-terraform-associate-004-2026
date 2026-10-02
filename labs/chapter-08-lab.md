# Chapter 8 Lab: Interact with Terraform Modules

**Estimated time:** 20 minutes

## Exercise

Create a root configuration that calls a local child module, passes input variables into it, and consumes one of its outputs in a root-level resource

## Key takeaway

Module sourcing and versioning rules differ by source type — version pinning only applies to Registry (and similar remote) sources, not local paths.
