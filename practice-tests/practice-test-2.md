# Practice Test 2: Terraform Associate (004) Certification Prep

- **Questions:** 44
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-2.md`. Track results in `score-tracker.md`.

---

**Q1.** A platform team enforces Terraform for all provisioning, but an engineer occasionally makes emergency changes directly in the cloud console to resolve incidents faster. Weeks later, `terraform plan` begins showing unexpected diffs even though no one edited the .tf files. Which IaC concept explains this behavior and the primary reason Terraform is designed to surface it?

A) Configuration drift — the real infrastructure no longer matches the declared state, and Terraform's plan compares real-world state against the desired configuration to reveal the gap
B) Idempotency failure — Terraform re-applied the same configuration twice and produced inconsistent results
C) Provider version skew — the console changes updated the provider plugin to a newer API version than the configuration expects
D) State locking failure — concurrent applies corrupted the state file, causing false diffs to appear

---

**Q2.** A company runs workloads across AWS, Azure, and an on-prem VMware cluster. Leadership wants a single team to manage provisioning for all three using one consistent operational workflow, even though the underlying APIs, authentication, and resource models differ completely between them. Which IaC benefit does this scenario primarily illustrate?

A) Terraform normalizes all three platforms into a single identical HCL schema, so the same resource blocks work unmodified across providers
B) State file portability — a single Terraform state file can represent resources from AWS, Azure, and VMware without separate backends
C) Automatic cloud migration — Terraform can convert AWS-specific resource definitions into Azure or VMware equivalents automatically
D) Provider-agnostic workflow — Terraform applies the same init/plan/apply/destroy lifecycle across any provider, even though each provider's HCL resources and arguments are unique

---

**Q3.** A new hire is used to writing imperative shell scripts that issue a sequence of CLI commands to create a VPC, then subnets, then an internet gateway, checking each step's output before running the next. Which IaC characteristic of Terraform's declarative model most directly removes the need for this kind of manual sequencing logic?

A) Terraform requires resources to be listed in the exact order they should be created, matching the script's original sequence
B) Terraform executes .tf files as a linear imperative script, top to bottom, in file order
C) Modules replace the need for dependency ordering entirely, since each module is provisioned independently in parallel
D) You declare the desired end state of resources and their dependencies, and Terraform's graph engine determines the correct creation order automatically

---

**Q4.** During a TA-004 exam-style discussion, two engineers debate terminology. One says Terraform is 'idempotent,' the other insists the correct term is that Terraform 'converges' infrastructure to a desired state. Which statement correctly distinguishes these two concepts as Terraform applies them?

A) They are synonyms in Terraform's context and can be used interchangeably with no meaningful distinction
B) Idempotency means running the same apply repeatedly with no drift produces no further changes; convergence describes Terraform calculating and applying only the delta needed to move current state toward desired state, regardless of the starting point
C) Convergence only applies to the first `terraform apply` on new infrastructure, while idempotency only applies to `terraform destroy` operations
D) Idempotency refers to state file locking behavior, while convergence refers to provider plugin version resolution during `terraform init`

---

**Q5.** A platform team provisions cloud infrastructure with Terraform and uses Ansible to configure software on the provisioned VMs after they exist. During a design review, a new engineer proposes replacing Ansible entirely with Terraform's remote-exec provisioner to handle all ongoing configuration management. Which statement BEST explains why the team should reject this proposal?

A) Terraform cannot connect to remote hosts over SSH or WinRM, so provisioners are non-functional for VM configuration
B) HashiCorp recommends provisioners as a last resort because they introduce imperative logic into an otherwise declarative dependency graph, but they can fully replace Ansible for ongoing configuration when written correctly
C) Ansible's agentless architecture makes it categorically faster than Terraform at any infrastructure task, so it should always be preferred
D) Terraform provisioners execute only during resource creation or destruction and are not designed for ongoing configuration management, so they cannot replace a dedicated CM tool for continuous drift correction

---

**Q6.** An organization is evaluating Terraform against a purely imperative scripting approach (a bash script that calls cloud CLI commands in a fixed sequence) for provisioning a VPC, subnets, and EC2 instances. Which capability is unique to Terraform's declarative model in this comparison?

A) Terraform automatically retries failed provider API calls up to a fixed number of times before failing the run
B) Terraform executes resource creation strictly in the order resources appear in the .tf files, exactly matching the imperative script's behavior
C) Terraform requires the same manually authored ordering logic as the bash script, but adds idempotency checks after each API call
D) Terraform builds a resource dependency graph from configuration references and determines the correct creation order automatically, without the author specifying explicit sequencing

---

**Q7.** A team needs to provision resources across AWS, Azure, and a private VMware vSphere environment from a single workflow, and wants one tool to track the current real-world state of all created resources so it can detect and reconcile drift on every run. Which factor makes Terraform the better fit for this requirement than a general-purpose configuration management tool like Ansible?

A) Terraform maintains a state file that models the last-known attributes of managed infrastructure and compares it against provider APIs on each plan, enabling drift detection independent of manually maintained inventories
B) Terraform is agentless, so it does not require SSH access to target systems, unlike Ansible
C) Ansible cannot connect to more than one cloud provider within a single playbook run
D) Terraform configuration files must be written in YAML, giving it a lower learning curve for teams already writing Ansible playbooks

---

**Q8.** During terraform plan, an engineer notices the CLI output shows an EC2 instance resource with '~ update in-place' for the instance_type attribute but '-/+ destroy and re-create' for the ami attribute within the same resource block. Which underlying Terraform behavior explains why the same resource shows two different change actions?

A) Terraform batches all attribute changes into a single action type per resource; this mixed output indicates a provider bug that must be reported to HashiCorp
B) The ami attribute always forces replacement across every Terraform provider, regardless of how that provider's schema defines it
C) Terraform only recreates a resource when its resource address changes in configuration, never based on attribute value changes
D) Each attribute in a resource's provider schema is marked as either updatable in-place or as forcing replacement (ForceNew); the plan reflects the most severe action required across all changed attributes for that resource

---

**Q9.** Your team's Terraform configuration currently pins the AWS provider to version 4.0.0. You want Terraform to automatically pick up new minor and patch releases (e.g., 4.1.0, 4.5.2) but must never allow the provider to upgrade to the 5.x major version line. Which version constraint should you use in the required_providers block?

A) ~> 4.0.0
B) ~> 4.0
C) >= 4.0.0
D) = 4.0.0

---

**Q10.** A teammate updated the version constraint for the azurerm provider in required_providers from "~> 3.0" to "~> 4.0" and pushed the change. When you pull the change and run 'terraform plan', Terraform reports that the installed provider version does not satisfy the new constraints and refuses to proceed, even though your .terraform.lock.hcl still lists the old 3.x version. What is the correct command to resolve this?

A) terraform state replace-provider
B) terraform validate -upgrade
C) terraform apply -auto-approve
D) terraform init -upgrade

---

**Q11.** You must deploy identical S3 buckets into both us-east-1 and eu-west-1 using a single AWS provider configuration block per region. You've defined a second aws provider block with alias = "euwest" pointing at eu-west-1. How do you tell the resource for the European bucket to use that specific provider configuration?

A) Add provider = aws.euwest inside the resource block
B) Add region = "eu-west-1" directly inside the resource block
C) Add alias = "euwest" inside the resource block
D) Reference it as var.aws.euwest inside the resource block

---

**Q12.** Your configuration needs both the official HashiCorp AWS provider and the community-maintained GitHub provider published under the 'integrations' namespace on the Terraform Registry. Which required_providers block correctly declares both?

A) terraform { required_providers { aws = { source = "hashicorp/aws", version = "~> 5.0" }, github = { source = "hashicorp/github", version = "~> 6.0" } } }
B) provider "aws" { source = "hashicorp/aws" } provider "github" { source = "integrations/github" }
C) terraform { required_providers { aws = { source = "hashicorp/aws", version = "~> 5.0" }, github = { source = "integrations/github", version = "~> 6.0" } } }
D) terraform { required_providers { aws = "hashicorp/aws", github = "integrations/github" } }

---

**Q13.** A resource managed by Terraform was manually deleted from the cloud console outside of Terraform's workflow. Your configuration for that resource is unchanged. What will 'terraform plan' show the next time it is run?

A) A plan to create the resource again, because state still records it as existing but a refresh shows it is now missing
B) No changes, because the configuration hasn't changed since the last apply
C) An error requiring you to manually run 'terraform state rm' before plan can proceed
D) A prompt asking whether to run 'terraform import' before continuing

---

**Q14.** Which statement correctly describes the relationship between the required_providers block and a provider configuration block in Terraform?

A) They serve the same purpose and are interchangeable; only one is needed per configuration
B) required_providers defines which provider plugins and version constraints are needed, while a provider block supplies the runtime configuration (like credentials or region) for a specific provider
C) required_providers configures credentials for the provider, while the provider block only pins the version
D) A provider block is only needed when using provider aliases; required_providers alone is sufficient for a single default provider configuration

---

**Q15.** A `terraform plan` shows: `-/+ aws_instance.web must be replaced` because the `ami` argument changed, and `ami` is a ForceNew attribute. Nothing else in the configuration changed. The team wants to apply only this replacement, without Terraform evaluating or touching any other resource in the state, and without editing any .tf files. Which command should they run?

A) terraform taint aws_instance.web && terraform apply
B) terraform apply -target="aws_instance.web"
C) terraform apply -replace="aws_instance.web"
D) terraform destroy -target="aws_instance.web" && terraform apply

---

**Q16.** An engineer wants to preview exactly what resources and attributes would be removed if `terraform destroy` were run — without actually deleting any infrastructure. Which command produces this preview?

A) terraform plan -destroy
B) terraform destroy -dry-run
C) terraform validate -destroy
D) terraform show -destroy

---

**Q17.** When you run `terraform apply` directly (without passing a previously saved plan file), what does Terraform do immediately before generating and displaying the plan for your approval?

A) It automatically runs terraform init to update provider plugins to their latest versions
B) It permanently locks the state file until a human manually releases the lock
C) It performs a refresh of the managed resources to reconcile the in-memory state with real infrastructure before computing the diff
D) It skips any state reconciliation and compares only the last-applied state to the configuration

---

**Q18.** A CI/CD pipeline needs a full, machine-readable JSON representation of a Terraform plan (for feeding into a policy-as-code tool like OPA) rather than just log output. Which command sequence is the correct, documented way to produce this?

A) terraform plan -json > plan.json
B) terraform plan --format=json > plan.json
C) terraform plan -out=tfplan && terraform show -json tfplan > plan.json
D) terraform state show -json > plan.json

---

**Q19.** An engineer wants to generate a visual representation (consumable by Graphviz) of the dependency order in which Terraform will create, update, or destroy resources, without applying any changes. Which command should they use?

A) terraform graph
B) terraform state graph
C) terraform plan -graph
D) terraform validate -graph

---

**Q20.** An engineer runs `terraform plan -out=tfplan`. Before they run `terraform apply tfplan`, a teammate applies an unrelated change directly through the cloud console, altering the real-world state of a resource referenced in that saved plan. What happens when the engineer then runs `terraform apply tfplan`?

A) Terraform applies the saved plan unconditionally, since a saved plan file bypasses all state checks
B) Terraform apply fails with an error indicating the saved plan is stale, and a new plan must be generated
C) Terraform silently merges the console change into the plan and applies both sets of changes together
D) Terraform prompts interactively, asking whether to proceed with the outdated plan or regenerate it

---

**Q21.** A Terraform configuration manages an EC2 instance. After changing the `ami` argument, `terraform plan` shows:

  # aws_instance.web must be replaced
  -/+ resource "aws_instance" "web" {
      ~ ami = "ami-0123" -> "ami-0456" # forces replacement
  }

The team needs the new instance to be created and pass health checks BEFORE the old instance is destroyed, to avoid downtime. Which approach BEST achieves this using the standard Terraform workflow?

A) Run `terraform apply -auto-approve` immediately so the replacement happens as fast as possible
B) Run `terraform taint aws_instance.web` before running plan, then apply as usual
C) Add a `lifecycle { create_before_destroy = true }` block to the resource, then run `terraform plan` and `terraform apply`
D) Run `terraform destroy -target=aws_instance.web` followed by `terraform apply` to recreate it

---

**Q22.** A module declares:

variable "instances" {
  type = list(object({
    name = string
    size = string
  }))
}

The team wants exactly one aws_instance resource created per list entry, addressable in state by the instance's `name` value (so reordering the list doesn't force replacements). Which for_each expression on the resource block satisfies this?

A) for_each = var.instances
B) for_each = { for i in var.instances : i.name => i }
C) for_each = toset(var.instances)
D) for_each = var.instances[*].name

---

**Q23.** An aws_security_group resource must generate a variable number of ingress rules from a list variable `var.ingress_rules`, each containing `port` and `cidr` fields. Which construct correctly generates these nested blocks?

A) ingress = [for r in var.ingress_rules : { from_port = r.port, to_port = r.port, cidr_blocks = [r.cidr] }]
B) dynamic "ingress" {
  for_each = var.ingress_rules
  content {
    from_port   = ingress.value.port
    to_port     = ingress.value.port
    cidr_blocks = [ingress.value.cidr]
  }
}
C) count = length(var.ingress_rules)
D) dynamic "ingress" {
  for_each = var.ingress_rules
  content {
    from_port   = each.value.port
    to_port     = each.value.port
    cidr_blocks = [each.value.cidr]
  }
}

---

**Q24.** Which Terraform block type does NOT support a `postcondition` (while supporting `precondition`)?

A) resource lifecycle block
B) data source lifecycle block
C) output value block
D) check block

---

**Q25.** A variable must only accept strings matching the pattern of a valid AWS instance type family, such as 't2.micro' or 'm5.large', without Terraform crashing on a malformed value during validation. Which validation condition is correct?

A) condition = can(regex("^[a-z][0-9]+\\.[a-z]+$", var.instance_type))
B) condition = regex("^[a-z][0-9]+\\.[a-z]+$", var.instance_type) == var.instance_type
C) condition = var.instance_type == regex("^[a-z][0-9]+\\.[a-z]+$")
D) condition = contains(regex("^[a-z][0-9]+\\.[a-z]+$", var.instance_type), var.instance_type)

---

**Q26.** A module output must return a value derived from a resource attribute marked sensitive by the provider, but the consuming root module needs to use that value as a map key elsewhere, which Terraform disallows for sensitive values. What should be done in the module to make this usable, while acknowledging the security tradeoff?

A) Add sensitive = true to the output block
B) Use jsonencode() on the value before returning it
C) Reference the value through a local instead of directly in the output
D) Wrap the value using nonsensitive() in the output's expression

---

**Q27.** A resource block references an output of a module only inside a `provisioner "local-exec"` command string that Terraform cannot statically parse for dependency purposes (e.g., embedded in a script argument built via templatefile()). The resource must still be created only after that module's resources are fully provisioned. What should be added?

A) Nothing — Terraform automatically infers the dependency from any string interpolation
B) A local value that maps the module output 1:1, replacing the need for depends_on
C) depends_on = [module.network] added to the resource block
D) lifecycle { create_before_destroy = true } on the resource

---

**Q28.** A module declares `variable "db_password" { type = string sensitive = true }` and includes `output "password_out" { value = var.db_password }` without a `sensitive` argument on the output. What happens when a caller runs `terraform plan` against the root module that consumes this module?

A) Terraform throws an error at plan time stating the output value is derived from a sensitive value and requires the output block to set `sensitive = true`
B) Terraform automatically infers the output is sensitive and redacts it in the CLI output with no configuration change needed
C) Terraform plan succeeds and prints the plaintext password to the console because outputs do not inherit sensitivity from variables
D) Terraform stores the value in plaintext in state but hides it from the CLI, requiring no changes to the output block

---

**Q29.** A platform team must source a reusable module from a private Git repository over SSH. The module is stored in the subdirectory 'modules/network' of that repository, and the team must pin their configuration to the tag v2.3.1. Which source configuration correctly achieves this?

A) source = "git::ssh://git@github.com/org/repo.git/modules/network?ref=v2.3.1"
B) source = "git::ssh://git@github.com/org/repo.git//modules/network"
version = "v2.3.1"
C) source = "github.com/org/repo//modules/network?ref=v2.3.1"
D) source = "git::ssh://git@github.com/org/repo.git//modules/network?ref=v2.3.1"

---

**Q30.** A module named 'vpc' is published to the public Terraform Registry under the namespace 'acme' for the 'aws' provider. Which module source string correctly references this module using registry shorthand?

A) "aws/vpc/acme"
B) "terraform-aws-modules/acme-vpc"
C) "acme/vpc/aws"
D) "hashicorp/acme/vpc"

---

**Q31.** A configuration consumes a registry module and must automatically pick up bug-fix (patch) releases within the 3.4.x line, but must NOT upgrade to 3.5.0 or any later minor/major release. Which version constraint enforces this?

A) version = "~> 3.4"
B) version = ">= 3.4.0, < 3.4.9"
C) version = "= 3.4.0"
D) version = "~> 3.4.0"

---

**Q32.** In the root module, 'module "network"' produces an output named 'subnet_ids'. The 'module "compute"' block requires an input variable called 'subnet_id_list' populated with that value. Which line correctly wires this inside the compute module block?

A) subnet_id_list = network.output.subnet_ids
B) subnet_id_list = var.network.subnet_ids
C) subnet_id_list = module.network.subnet_ids
D) subnet_id_list = data.network.subnet_ids

---

**Q33.** A root module declares:

module "app" {
  for_each = toset(["dev", "staging", "prod"])
  source   = "./modules/app"
  env_name = each.key
}

Which expression correctly references the 'url' output of the 'prod' instance elsewhere in the same root module?

A) module.app["prod"].url
B) module.app.prod.url
C) module.app[2].url
D) module.app.url["prod"]

---

**Q34.** A team enables state locking for an S3 backend but wants to avoid provisioning a separate DynamoDB table. They are running Terraform 1.10. Which backend configuration argument enables native S3-based locking using conditional writes instead of DynamoDB?

A) dynamodb_table = "terraform-locks"
B) lock_table = "terraform-locks"
C) enable_native_lock = true
D) use_lockfile = true

---

**Q35.** A resource attribute containing a database password is marked sensitive = true in an output block, but a teammate later opens the raw state file and finds the plaintext value. What is the correct explanation and remediation?

A) Marking a value sensitive encrypts it inside the state file; the teammate must have used an outdated Terraform version
B) Run terraform state rm on the resource immediately after apply to purge the value from state
C) The sensitive flag only suppresses the value from CLI output and the UI — it does not encrypt state; store state in a backend with encryption at rest and restrict access via IAM policy
D) Replace the resource with a null_resource wrapper so the attribute is never written to state

---

**Q36.** A team is renaming a resource across a module used by 12 CI-driven environments (workspaces). They need the change to apply consistently and automatically the next time each environment's pipeline runs terraform plan, without any engineer needing to run a manual command against every workspace. Which approach BEST satisfies this requirement?

A) Have each environment's engineer run terraform state mv manually before the next apply
B) Add a moved block to the configuration mapping the old address to the new one
C) Run terraform import against each workspace with the new resource address
D) Delete the resource from state with terraform state rm and let Terraform recreate it under the new name

---

**Q37.** A resource block is deleted from configuration, but the team wants Terraform to stop managing the corresponding real-world infrastructure WITHOUT destroying it, and wants this decision version-controlled so every teammate and CI pipeline applies it consistently. Which approach BEST meets these requirements?

A) Have each engineer run terraform state rm on their own machine before applying
B) Add a moved block pointing the old address to a new address that doesn't exist
C) Add a removed block referencing the resource with a lifecycle { destroy = false } setting
D) Run terraform apply with the -refresh-only flag to detach the resource

---

**Q38.** What does running terraform plan -refresh-only actually produce?

A) A proposed set of configuration changes to bring real infrastructure into alignment with the current .tf files
B) A report showing differences between the current state file and the real infrastructure, without proposing any configuration-driven changes
C) An automatic update of the state file to match real infrastructure, applied immediately without confirmation
D) A refreshed list of available provider plugin versions compatible with the configuration

---

**Q39.** A team runs terraform import aws_instance.web i-0abc123456 against a configuration that has no aws_instance.web resource block defined anywhere in the .tf files. What is the result?

A) Terraform automatically generates a matching HCL resource block for aws_instance.web based on the imported attributes
B) Terraform errors out, stating the resource address does not exist in the configuration, because the classic import command requires a pre-existing resource block
C) The import succeeds, and the next terraform plan will show the resource must be destroyed since no config matches it
D) The import silently fails and the state file remains completely unchanged with no error message

---

**Q40.** A platform team manages 40 HCP Terraform workspaces provisioning AWS resources for different application teams. Security requires that no long-lived AWS access keys be stored in HCP Terraform, and that each workspace can only assume the IAM role scoped to its own environment. Which HCP Terraform feature should they configure to meet this requirement?

A) Dynamic provider credentials using OIDC federation with per-workspace assigned IAM roles
B) Variable sets applied at the organization level containing a shared AWS access key
C) A Sentinel policy that blocks 'terraform apply' if any variable name contains the word 'secret'
D) Run triggers that pass credentials from a central 'vault' workspace's state outputs

---

**Q41.** A company wants a Sentinel enforcement level where a failing policy check permanently blocks 'terraform apply' for every run, with no way for any user, including organization owners, to override the failure at run time (the only way to allow the run is to edit or remove the policy itself). Which enforcement level satisfies this?

A) advisory
B) hard-mandatory
C) soft-mandatory
D) admin-override

---

**Q42.** An organization has 15 workspaces that all deploy into the same AWS account and need identical AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, and a shared 'cost-center' Terraform variable. The platform team wants to manage these values in a single place and have updates automatically propagate to all 15 workspaces without editing each workspace individually. Which HCP Terraform feature should they use?

A) A shared Terraform module that hardcodes the variable values
B) A variable set scoped to the relevant workspaces (or the entire organization)
C) Workspace-level variables manually copied into each of the 15 workspaces
D) A run trigger chaining all 15 workspaces together sequentially

---

**Q43.** Workspace 'network' provisions a shared VPC. Workspace 'app' deploys application infrastructure into that VPC and must automatically queue a new run whenever 'network' successfully applies changes. Which configuration should be added to workspace 'app' in HCP Terraform?

A) A run trigger in 'app' with 'network' set as the source workspace
B) A Sentinel policy in 'app' that reads the remote state of 'network'
C) A VCS webhook configured on the 'network' repository pointing to 'app'
D) A matching workspace tag applied to both 'network' and 'app'

---

**Q44.** An engineer enables health assessments (drift detection) on an HCP Terraform workspace, scheduled to run every 24 hours. What happens when a scheduled health assessment discovers that real infrastructure has drifted from the Terraform state?

A) HCP Terraform automatically runs 'terraform apply' to reconcile the drift without requiring approval
B) HCP Terraform removes the drifted resources from the state file to force recreation on the next apply
C) HCP Terraform performs a refresh-only plan and reports the drifted resources in the workspace UI without applying any changes
D) HCP Terraform locks the workspace and blocks all new runs until the drift is manually resolved

---

