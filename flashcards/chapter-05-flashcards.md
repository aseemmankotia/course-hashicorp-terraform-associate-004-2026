## Chapter 5: Core Workflow Part 2: Apply, Destroy, Fmt & the Resource Graph — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Default parallelism for terraform apply operations? | 10 concurrent resource operations. Change with -parallelism=n on plan/apply. Exam tip: this controls concurrency, not order -- order is still determined by the dependency graph. |
| 2 | terraform apply <saved-plan-file> -- does it prompt for confirmation? | No. Applying a saved plan file skips the interactive yes/no prompt since approval already happened at plan-review time. Exam tip: -auto-approve is redundant here. |
| 3 | terraform apply with no saved plan file -- what confirmation is required? | Terraform prompts: 'Only "yes" will be accepted to approve.' Any other input (including 'y') cancels the apply. |
| 4 | -auto-approve flag risk in CI/CD pipelines? | Skips the interactive approval prompt, allowing unreviewed (potentially destructive) changes to apply automatically with no human checkpoint. Best practice: pair with a reviewed plan-file stage, not raw inline apply. |
| 5 | terraform destroy -target=<resource> -- official guidance? | HashiCorp documents -target for exceptional/recovery circumstances only. Routine use risks leaving state inconsistent with real infrastructure since dependency handling is limited. |
| 6 | terraform destroy without -target -- scope? | Destroys ALL resources tracked in the current state/workspace, not just a subset. Exam gotcha: no confirmation prompt is skipped by default -- destroy still asks for 'yes'. |
| 7 | terraform fmt -- what does it check/change? | Rewrites configuration files into canonical style (indentation, spacing, alignment) ONLY. It does NOT validate logic, syntax correctness, or provider schema. |
| 8 | terraform fmt vs terraform validate? | fmt = style/formatting only. validate = checks syntax and internal consistency (required args, type errors) without touching providers or state. Different jobs -- exam tests confusing these. |
| 9 | terraform graph -- purpose and output format? | Outputs the resource dependency graph in DOT format, describing execution order Terraform uses for apply/destroy. Typically piped to Graphviz (e.g., `terraform graph \| dot -Tsvg > graph.svg`) for visualization. |
| 10 | Does terraform graph require an extra tool to visualize? | Yes -- Terraform only emits raw DOT text; rendering an image requires Graphviz (or a DOT-compatible viewer). |
| 11 | How to change Terraform's apply/destroy concurrency limit? | Use the -parallelism=n flag on terraform plan, apply, or destroy. Default is 10. Lower it to reduce API rate-limit errors against a cloud provider. |
| 12 | Exam trap: -target usage scope | -target=<resource> only limits operations to that resource AND its dependencies -- it does not guarantee full consistency across the rest of the config, which is why HashiCorp restricts recommended use to recovery scenarios. |
