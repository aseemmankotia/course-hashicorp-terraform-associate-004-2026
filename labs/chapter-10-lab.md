# Chapter 10 Lab: Drift, Refactoring State & Import

**Estimated time:** 25 minutes

## Exercise

Simulate drift by manually modifying a resource outside Terraform, run terraform plan -refresh-only to detect it, then use a moved block to rename a resource in configuration without triggering destroy/recreate

## Key takeaway

moved, removed, refresh-only, and import each solve a distinct state-refactoring problem — pick based on whether infrastructure or just state bookkeeping needs to change.
