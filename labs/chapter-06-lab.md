# Chapter 6 Lab: Writing HCL: Resources, Data Sources & Dependencies

**Estimated time:** 25 minutes

## Exercise

Write a configuration with one resource and one data source, create an implicit dependency by referencing the resource's attribute in another resource, then add an explicit depends_on where no attribute reference exists

## Key takeaway

Attribute references create implicit dependencies automatically; depends_on is only for the rare cases Terraform can't infer on its own.
