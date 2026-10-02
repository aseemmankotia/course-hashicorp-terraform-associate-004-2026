# Chapter 9 Lab: Terraform State Fundamentals & Remote Backends

**Estimated time:** 20 minutes

## Exercise

Configure a backend block for remote state using a generic example backend, run terraform init to migrate local state, then use terraform state list and terraform state show to inspect resources

## Key takeaway

State maps config to real infrastructure; remote backends with locking are essential once more than one person or process touches the same state.
