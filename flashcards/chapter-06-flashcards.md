## Chapter 6: Writing HCL: Resources, Data Sources & Dependencies — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Resource vs data block: which one can create/destroy infrastructure? | resource block manages full lifecycle (create/update/destroy). data block is read-only, never creates or destroys. Exam tip: data blocks are re-read on every plan/apply. |
| 2 | Syntax to reference an attribute from a resource block | resource_type.resource_name.attribute — e.g. aws_instance.web.id. This reference alone creates an implicit dependency; no depends_on needed. |
| 3 | Syntax to reference an attribute from a data block | data.data_type.data_name.attribute — e.g. data.aws_ami.latest.id. Note the extra 'data.' prefix that resource references don't have. |
| 4 | When is depends_on actually required in Terraform? | Only when a real dependency exists but is NOT visible through any attribute reference (a hidden/behavioral dependency), e.g. an IAM policy must exist before an EC2 instance uses the role, with no config link between them. |
| 5 | depends_on value type and correct syntax | A list of resource or module references: depends_on = [aws_iam_role_policy.example]. Exam trap: takes the resource reference itself, NOT a specific attribute (.id) and NOT a string. |
| 6 | Terraform destroy order for dependent resources | Reverse of creation order — dependents are destroyed before their dependencies. If B references A, B is destroyed first, then A. |
| 7 | variable block arguments (optional/recommended) | type (recommended), default (optional — makes the variable optional), description (good practice), validation (optional constraint), sensitive (optional, hides value from CLI output). |
| 8 | output block sensitive argument — what does it actually hide? | sensitive = true hides the value from CLI output (plan/apply console). Exam trap: it does NOT encrypt or remove the value from the state file — state still stores it in plaintext. |
| 9 | How to bring an existing manually-created resource under Terraform management? | Write the matching resource block, then run terraform import <resource_address> <id>. A data block alone will NOT let Terraform manage or destroy it. |
| 10 | variable validation block structure | validation { condition = <bool expr using var.name>; error_message = "string" }. Runs at plan time, before any resource changes are proposed. |
| 11 | Does referencing a data source create an implicit dependency like a resource reference does? | Yes — referencing data.TYPE.NAME.attribute inside a resource creates an implicit dependency, using the same mechanism as resource-to-resource references. |
| 12 | Data source refresh behavior | Data sources are read/refreshed during every plan and apply, always reflecting current real infrastructure — unlike resources, which only change when you apply a config change. |
