## Chapter 5: Core Workflow Part 2: Apply, Destroy, Fmt & the Resource Graph — Practice Questions

**Q1.** A team runs `terraform plan -out=tfplan` in CI, then a manual review step, then a separate CI job runs `terraform apply tfplan`. What happens during the apply step?

A) Terraform re-evaluates the configuration and prompts for confirmation before applying
B) Terraform fails because saved plan files cannot be applied in a different CI job
C) Terraform applies the exact saved plan without generating a new plan and without an interactive approval prompt
D) Terraform requires -auto-approve to be passed alongside the plan file or it will error out

<details>
<summary>Answer</summary>

**Correct: C**

Applying a saved plan file (terraform apply tfplan) executes exactly what was planned and does not prompt for interactive approval, since approval is implicit in choosing to apply that specific plan file.

**Why the others are wrong:**
- A saved plan is not re-evaluated at apply time; the point of saving a plan is to apply that exact set of actions
- Saved plan files are portable and commonly applied in a separate CI stage/job
- -auto-approve only affects inline (non-file) applies where a prompt would otherwise appear; it is not required for plan-file applies

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A junior engineer adds -auto-approve to a `terraform apply` step that runs on every merge to main, without any plan file review step. What is the PRIMARY risk this introduces?

A) Unreviewed infrastructure changes (including destructive ones) can be applied automatically with no human checkpoint
B) The apply will run slower because auto-approve disables parallelism
C) Terraform will refuse to apply and require manual confirmation anyway
D) The state file will be locked permanently after the apply completes

<details>
<summary>Answer</summary>

**Correct: A**

-auto-approve suppresses the interactive yes/no confirmation, so any change the plan produces -- including deletions or replacements -- is applied immediately without a human reviewing it first, which is the main exam-tested risk in pipelines.

**Why the others are wrong:**
- -auto-approve has no effect on parallelism settings
- -auto-approve's entire purpose is to bypass the confirmation prompt, not trigger it
- State locking is unrelated to the -auto-approve flag

</details>

---

**Q3.** A single broken resource is blocking `terraform apply` for an entire configuration, and the team needs to destroy just that one resource quickly. Which command accomplishes this, and what is the documented caution around it?

A) terraform destroy --resource=<resource>; it is fully safe for routine day-to-day use
B) terraform apply -destroy=<resource>; it automatically updates all dependent resources safely
C) terraform destroy -target=<resource>; it is documented for exceptional/recovery use only, since it can produce an incomplete or inconsistent state if used routinely
D) terraform state rm <resource>; this destroys the resource in the cloud provider

<details>
<summary>Answer</summary>

**Correct: C**

terraform destroy -target=<resource> limits destroy operations to the named resource (and its dependents), but HashiCorp explicitly documents -target for exceptional circumstances like recovery, because it can leave state inconsistent with real infrastructure if relied on routinely.

**Why the others are wrong:**
- --resource is not a valid Terraform flag, and -target is never described as fully safe for routine use
- -destroy is not a valid flag on terraform apply for targeting single resources this way
- terraform state rm only removes a resource from state tracking; it does not destroy the actual infrastructure

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** What does `terraform fmt` actually validate or change in a configuration?

A) It rewrites files into the canonical style and formatting convention; it does not check logical correctness or provider syntax validity
B) It checks that all required provider arguments are present and errors if any are missing
C) It validates that resource dependencies form a valid, non-cyclic graph
D) It formats files and also runs a full plan to confirm the configuration applies cleanly

<details>
<summary>Answer</summary>

**Correct: A**

terraform fmt only rewrites configuration files into Terraform's canonical style (indentation, alignment, spacing) -- it performs no logic, syntax, or provider validation.

**Why the others are wrong:**
- Checking required arguments and configuration correctness is the job of terraform validate, not fmt
- Detecting dependency cycles is part of graph construction during plan/apply, not fmt
- fmt never runs a plan or apply; it is purely a style tool

</details>

---

**Q5.** Which command produces a visual representation of the resource dependency graph Terraform uses internally to determine apply/destroy order?

A) terraform graph
B) terraform plan -graph
C) terraform show -dependencies
D) terraform state graph

<details>
<summary>Answer</summary>

**Correct: A**

terraform graph outputs the resource dependency graph in DOT format, which can be piped into a tool like Graphviz to render a visual diagram of dependency and execution order.

**Why the others are wrong:**
- -graph is not a valid flag on terraform plan
- terraform show -dependencies does not exist; terraform show renders state or plan content, not a graph
- terraform state graph is not a valid subcommand

</details>

---

**Q6.** By default, how many resource operations can Terraform perform concurrently during terraform apply, and how is this changed?

A) 1, and it cannot be changed
B) 10, changed via the -parallelism=n flag on plan/apply
C) 5, changed via TF_PARALLELISM environment variable only
D) Unlimited, changed via -parallelism=0

<details>
<summary>Answer</summary>

**Correct: B**

Terraform defaults to a parallelism of 10 concurrent resource operations during apply, and this value can be overridden with the -parallelism=n flag on terraform plan or terraform apply.

**Why the others are wrong:**
- Terraform does not default to serial (1) execution -- it parallelizes independent resources up to the limit
- There is no dedicated TF_PARALLELISM environment variable; the flag is the supported mechanism
- Parallelism is not unlimited by default, and -parallelism=0 does not mean unlimited

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** An engineer runs `terraform apply -auto-approve tfplan` where tfplan is a previously saved plan file. What is the practical effect of including -auto-approve in this command?

A) It causes an error because -auto-approve cannot be combined with a saved plan file
B) It has no meaningful effect since applying a saved plan file already skips the interactive confirmation prompt
C) It doubles the parallelism used during apply
D) It forces Terraform to re-plan before applying, ignoring the saved plan

<details>
<summary>Answer</summary>

**Correct: B**

Applying a saved plan file never shows an interactive approval prompt in the first place, so -auto-approve is redundant in this context -- a common exam trap testing whether students understand when the flag actually matters.

**Why the others are wrong:**
- Combining -auto-approve with a plan file is valid syntax, not an error
- -auto-approve has no relationship to parallelism settings
- A saved plan file is applied as-is; Terraform does not silently re-plan when a plan file argument is given

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the key conceptual difference between terraform fmt and terraform validate?

A) fmt enforces canonical code style/formatting, while validate checks configuration syntax and internal consistency (e.g., required arguments, type errors)
B) fmt checks provider credentials while validate checks state file integrity
C) fmt and validate are aliases for the same underlying command
D) fmt runs only in CI while validate runs only locally

<details>
<summary>Answer</summary>

**Correct: A**

terraform fmt is purely cosmetic -- it rewrites files to a standard style. terraform validate performs a syntax and internal consistency check of the configuration (without touching state or providers' remote data), catching errors like missing required arguments or type mismatches.

**Why the others are wrong:**
- Neither command checks provider credentials or state file integrity
- They are distinct commands with entirely different purposes, not aliases
- Both commands can be run in any environment, local or CI; there is no such restriction

</details>

---

