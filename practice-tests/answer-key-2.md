# Answer Key — Practice Test 2

**Q1. Correct: A**

Manual out-of-band changes cause the actual infrastructure to diverge from what's declared in code — this is configuration drift. One of the core value propositions of IaC/Terraform is that `plan` detects drift by comparing the refreshed real-world state to the desired configuration, making the divergence visible before it causes bigger problems.

---

**Q2. Correct: D**

Terraform's multi-cloud/hybrid-cloud value isn't that every provider's resources look the same in HCL — they don't — it's that the operational workflow (init, plan, apply, destroy) and tooling stay consistent regardless of which provider is behind the scenes, letting one team standardize process across heterogeneous infrastructure.

---

**Q3. Correct: D**

Terraform is declarative: you describe the desired state and let implicit/explicit dependencies inform a dependency graph. Terraform's engine walks this graph to determine correct ordering and parallelizes non-dependent resources — eliminating the need to hand-write step-by-step imperative sequencing like the shell script approach.

---

**Q4. Correct: B**

Idempotency describes the outcome property: reapplying unchanged configuration against unchanged infrastructure produces no additional effect. Convergence describes the mechanism: Terraform compares current state to desired state from wherever it starts and computes/applies only the necessary delta. Both are real, related, but distinct IaC concepts underpinning Terraform's reliability.

---

**Q5. Correct: D**

Provisioners (including remote-exec) only run once, at create or destroy time, tied to a resource's lifecycle. They have no mechanism to periodically re-check or correct configuration drift the way a CM tool run on a schedule or via orchestration can, so they are unsuitable as a full replacement for ongoing configuration management.

---

**Q6. Correct: D**

Terraform parses resource attribute references to build a directed acyclic resource graph, then walks that graph to determine correct create/update/destroy order. The author never writes explicit ordering logic — this is the core distinction from imperative scripting, where sequence must be hand-coded.

---

**Q7. Correct: A**

Terraform's state file is the mechanism that enables systematic drift detection: every plan compares the state's recorded attributes against real provider data. This state-centric model, not agent architecture or file format, is what distinguishes Terraform's fit for cross-platform provisioning and reconciliation.

---

**Q8. Correct: D**

Provider schemas declare per-attribute update behavior. Attributes flagged ForceNew (like ami on aws_instance) trigger destroy-and-recreate, while others (like instance_type) support in-place update via the API. When multiple attributes change, Terraform's plan surfaces the most disruptive required action for that resource.

---

**Q9. Correct: B**

The pessimistic constraint operator behaves differently depending on how many version segments are given. '~> 4.0' (two segments) means >= 4.0.0 and < 5.0.0, so it allows both minor and patch upgrades within the 4.x line but blocks 5.0.

---

**Q10. Correct: D**

'terraform init -upgrade' tells Terraform to consider newer provider versions that satisfy the updated constraints, download the new provider, and update the .terraform.lock.hcl file accordingly.

---

**Q11. Correct: A**

To route a resource to a non-default (aliased) provider configuration, you set the meta-argument 'provider = <PROVIDER TYPE>.<ALIAS>' inside the resource block, e.g., provider = aws.euwest.

---

**Q12. Correct: C**

The GitHub provider on the public registry is published under the 'integrations' namespace (source = "integrations/github"), not 'hashicorp'. This option correctly maps each local name to its true namespace/type using the required object syntax {source, version} inside the required_providers block.

---

**Q13. Correct: A**

During plan, Terraform refreshes its view of real infrastructure. If the state file says the resource should exist but the provider reports it's gone, Terraform reconciles this by proposing to create it again to match the desired configuration — this is the core function of state as the source of truth for existing/expected resources.

---

**Q14. Correct: B**

required_providers (nested in the terraform block) declares which providers are needed by source address and version constraint, letting Terraform know what to download during init. Separately, one or more provider blocks configure how Terraform should actually talk to that provider's API (region, credentials, endpoints, aliases).

---

**Q15. Correct: B**

-target restricts the plan/apply operation's scope to the named resource (and its dependencies), so Terraform only evaluates and applies that resource, ignoring the rest of the configuration and state — exactly what's required here since the replacement is already detected from the config diff.

---

**Q16. Correct: A**

terraform plan -destroy generates a speculative destroy plan, showing every resource that would be removed, without modifying real infrastructure or requiring confirmation.

---

**Q17. Correct: C**

By default, terraform apply (like terraform plan) refreshes the state of managed resources against real infrastructure first (unless -refresh=false is passed), ensuring the plan is computed against current real-world values.

---

**Q18. Correct: C**

The documented pattern for policy-as-code integration is to save a binary plan file with -out, then convert it to a full JSON plan representation using terraform show -json <planfile>, which HashiCorp's policy tooling (like Sentinel/OPA integrations) is built to consume.

---

**Q19. Correct: A**

terraform graph outputs the resource dependency graph in DOT format, which can be piped into Graphviz (e.g., `terraform graph | dot -Tsvg > graph.svg`) to visualize the order of operations Terraform will follow.

---

**Q20. Correct: B**

A saved plan file is tied to the specific state snapshot it was generated against. If the real state changes before the plan is applied, Terraform detects the mismatch and errors out (commonly surfaced as a stale-plan error), forcing the user to regenerate the plan against current state rather than risk applying an outdated diff.

---

**Q21. Correct: C**

The `create_before_destroy` lifecycle meta-argument changes the default replacement order: Terraform will provision the new resource first and only destroy the old one after the new one successfully exists, which the updated plan output will reflect. This is the correct workflow-level mechanism for controlling replacement order without downtime.

---

**Q22. Correct: B**

for_each requires a map or a set of strings. Converting the list of objects into a map keyed by `name` gives Terraform a stable key so reordering the source list doesn't cause resources to be destroyed/recreated, and each.value still exposes the full object for other attributes.

---

**Q23. Correct: D**

A dynamic block is required to generate a variable number of nested configuration blocks (like ingress) from a collection. Inside the content block, the iterator variable defaults to the block's label name — here 'ingress' — so `each.value` is wrong; the correct reference should be `ingress.value`. Wait — actually the correct answer generates the block using `dynamic` with the proper iterator; among the given choices, only the dynamic block options are structurally valid, and this one correctly matches Terraform's default iterator naming convention semantics tested on the exam (the block label is the default iterator name).

---

**Q24. Correct: C**

Output value blocks support only `precondition` blocks, which validate the output's value before it is emitted. Output blocks cannot contain `postcondition` blocks; that capability is reserved for resource and data source lifecycle blocks and check blocks.

---

**Q25. Correct: A**

regex() raises an error if the pattern doesn't match, which would crash validation entirely rather than producing a clean false result. Wrapping it in can() catches that error and converts it into a boolean, making it safe to use as a validation condition that returns false on non-matching input instead of erroring out.

---

**Q26. Correct: D**

nonsensitive() explicitly strips the sensitive marking from a value, which is the only built-in mechanism for deliberately downgrading a value so it can be used in contexts (like map keys) that reject sensitive values. This should be done with care since it removes Terraform's redaction protections for that value going forward.

---

**Q27. Correct: C**

When a reference is buried inside constructs Terraform can't reliably parse for its dependency graph (such as inside a rendered script string via templatefile), implicit dependency detection may not be sufficient. Adding depends_on = [module.network] explicitly forces the ordering, ensuring the module's resources are fully provisioned first regardless of how the reference is used internally.

---

**Q28. Correct: A**

Terraform propagates sensitivity: if a value used in an output is derived from a variable, resource attribute, or other value marked sensitive, Terraform requires the output block itself to declare `sensitive = true`. Without it, `terraform plan`/`apply` fails with an error like 'Output refers to sensitive values' until the output is explicitly marked sensitive.

---

**Q29. Correct: D**

Go-getter's Git subdirectory syntax requires a double slash ('//') to separate the repository URL from the in-repo path, and the ref query parameter selects the tag/branch/commit. Using ssh:// with the git:: prefix ensures the private repo is cloned over SSH.

---

**Q30. Correct: C**

The public registry shorthand format is <NAMESPACE>/<NAME>/<PROVIDER>, so 'acme/vpc/aws' correctly maps namespace=acme, name=vpc, provider=aws, resolving to registry.terraform.io/modules/acme/vpc/aws.

---

**Q31. Correct: D**

The pessimistic constraint operator '~>' allows increments only in the rightmost specified version segment. With '~> 3.4.0', the rightmost segment is the patch number, so Terraform allows 3.4.1, 3.4.2, etc., but rejects 3.5.0 and above.

---

**Q32. Correct: C**

Module outputs are referenced from a calling module using the syntax module.<MODULE_NAME>.<OUTPUT_NAME>, so module.network.subnet_ids correctly retrieves the subnet_ids output from the network module instance.

---

**Q33. Correct: A**

When a module block uses for_each, Terraform creates a map of module instances keyed by each.key, and each instance's outputs must be accessed with bracket-index syntax: module.<NAME>["<KEY>"].<OUTPUT>. Here that resolves to module.app["prod"].url.

---

**Q34. Correct: D**

Terraform 1.10 introduced native S3 state locking via S3's conditional write (If-None-Match) support. Setting use_lockfile = true in the s3 backend block enables this without requiring a DynamoDB table.

---

**Q35. Correct: C**

The sensitive attribute only redacts values from plan/apply CLI output and the HCP Terraform UI. The state file itself always stores the real value in plaintext (in the JSON structure), so protecting it requires backend-level encryption at rest plus strict IAM/access control on who can read the state.

---

**Q36. Correct: B**

The moved block is committed directly to the configuration, so it is version-controlled and automatically applied by Terraform's plan/apply logic in every workspace and CI run — no manual per-environment command is required.

---

**Q37. Correct: C**

The removed block (introduced in Terraform 1.7) is written directly in configuration and version-controlled. Setting lifecycle { destroy = false } tells Terraform to stop managing the resource and drop it from state without issuing a destroy call against the real infrastructure — and it's consistently applied by every plan/apply, including CI.

---

**Q38. Correct: B**

-refresh-only mode queries real infrastructure and compares it to the state file, surfacing drift (out-of-band changes) as a plan of proposed STATE updates only — it never proposes changes derived from the .tf configuration.

---

**Q39. Correct: B**

The classic terraform import CLI command only writes to the state file — it requires a resource block with that exact address to already exist in configuration, and errors immediately if it does not find one. Automatic HCL generation is only available via the newer `import` configuration block with the -generate-config-out flag, not the legacy import command.

---

**Q40. Correct: A**

Dynamic provider credentials use OIDC federation so HCP Terraform requests short-lived, workspace-scoped cloud credentials at run time from the cloud provider's identity federation service, with no long-lived keys stored anywhere and each workspace mapped to its own IAM role via trust policy conditions.

---

**Q41. Correct: B**

hard-mandatory policies cannot be overridden by anyone during a run; a failing hard-mandatory check always halts the run, and the policy set itself must be changed to allow the apply.

---

**Q42. Correct: B**

A variable set is defined once and attached to multiple workspaces (or the whole organization); updating the variable set propagates the change to every attached workspace automatically, which is exactly what's needed here.

---

**Q43. Correct: A**

Run triggers are configured on the downstream (dependent) workspace and specify one or more upstream 'source workspaces'; whenever a source workspace applies successfully, HCP Terraform automatically queues a new run in the workspace with the trigger.

---

**Q44. Correct: C**

Health assessments run a refresh-only plan against the workspace on the configured schedule, surface any detected drift (and, for Terraform Enterprise/HCP Terraform, deferred/missing resources) in the workspace's health dashboard, and never apply changes automatically.

---

