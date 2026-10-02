# Chapter 5 Lab: Core Workflow Part 2: Apply, Destroy, Fmt & the Resource Graph

**Estimated time:** 20 minutes

## Exercise

Run terraform apply against a saved plan file, then run terraform destroy -target on a single resource, and generate a terraform graph output to visualize dependency order

## Key takeaway

apply executes the reviewed plan, destroy removes resources (use -target sparingly), and fmt/graph support style and visualization, not logic checks.
