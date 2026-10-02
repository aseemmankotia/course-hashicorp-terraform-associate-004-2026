# Chapter 3 Lab: Terraform Fundamentals: Providers and State Basics

**Estimated time:** 20 minutes

## Exercise

Write a required_providers block pinning the hashicorp/random provider to a ~> constraint, run terraform init, inspect the generated .terraform.lock.hcl, then add a second aliased provider block

## Key takeaway

Version constraints plus a committed lock file keep provider versions consistent across a team, while state maps config to real infrastructure.
