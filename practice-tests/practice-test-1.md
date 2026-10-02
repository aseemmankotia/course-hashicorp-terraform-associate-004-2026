# Practice Test 1: Terraform Associate (004) Certification Prep

- **Questions:** 44
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** A company's operations team manually configures each new server through ad-hoc SSH sessions. Over time, servers that were supposed to be identical have started behaving differently, and nobody can explain why. Which IaC advantage BEST addresses this problem?

A) Cost optimization through pay-as-you-go billing
B) Declarative, version-controlled configuration that eliminates configuration drift
C) Real-time monitoring and alerting for infrastructure health
D) Automatic scaling based on traffic patterns

---

**Q2.** A company needs to provision resources across AWS, Azure, and an on-premises VMware cluster using a single configuration language and workflow. Which Terraform capability makes this possible?

A) Terraform's built-in cloud-specific SDKs bundled directly into Terraform Core for AWS and Azure only
B) Terraform's provider plugin architecture, which lets one core engine manage resources across multiple platforms
C) Terraform Cloud's mandatory SaaS billing model
D) Terraform's requirement that configuration be rewritten in each vendor's native templating language

---

**Q3.** A platform team is choosing between manually documenting server setup steps in a wiki versus defining infrastructure as code artifacts stored in a Git repository. Which advantage does the team gain by choosing the code-based approach?

A) Guaranteed zero-downtime deployments for every change
B) Repeatable, auditable infrastructure changes through version control history
C) Elimination of the need for any testing before deployment
D) Automatic conversion of legacy infrastructure to cloud-native services

---

**Q4.** Which statement BEST describes a core problem that infrastructure as code is designed to solve?

A) The lack of consistent, repeatable processes when infrastructure is provisioned and modified manually
B) The need to rewrite application code for each new cloud provider
C) The requirement for infrastructure teams to use only open-source software
D) The inability of virtual machines to run containerized workloads

---

**Q5.** A platform team currently uses shell scripts to imperatively call cloud provider APIs to create VMs, networks, and load balancers, one command at a time. They want to move to a tool that describes the desired end state of infrastructure, automatically determines the order of operations, and shows a preview of changes before anything is applied. Which approach BEST meets these requirements?

A) Continue using shell scripts but add version control
B) Use Ansible playbooks with imperative tasks executed in sequence
C) Use a bash-based CI/CD pipeline calling cloud CLI commands in parallel
D) Adopt Terraform's declarative configuration and run `terraform plan` before every apply

---

**Q6.** An organization is deciding how to divide responsibilities between Terraform and Ansible when managing both the provisioning of new AWS EC2 instances and the ongoing installation of packages on those instances. Which statement BEST describes how the two tools typically complement each other in this scenario?

A) Terraform should handle package installation while Ansible provisions the underlying infrastructure
B) Terraform is best suited for provisioning and managing infrastructure resources via state, while Ansible is best suited for configuration management tasks on top of that infrastructure
C) Terraform and Ansible are interchangeable and either can fully replace the other for both tasks
D) Ansible should be used exclusively because it does not require maintaining a state file

---

**Q7.** What is the primary purpose of Terraform's resource dependency graph?

A) To provide a graphical dashboard for monitoring live infrastructure metrics
B) To determine the correct order to create, update, or destroy resources based on their dependencies
C) To store the state of all deployed resources in a single file
D) To enforce role-based access control policies on HCP Terraform workspaces

---

**Q8.** Which Terraform command allows engineers to preview the exact changes that will be made to infrastructure before those changes are actually applied?

A) terraform validate
B) terraform graph
C) terraform state list
D) terraform plan

---

**Q9.** Your team wants the AWS provider to automatically pick up patch-level bug fixes (e.g., 5.10.1, 5.10.2) but never accept a minor or major version bump like 5.11.0 or 6.0.0. Which version constraint should you use in the required_providers block?

A) version = ">= 5.10.0"
B) version = "~> 5.10.0"
C) version = "= 5.10.0"
D) version = "~> 5.10"

---

**Q10.** What is the primary purpose of the .terraform.lock.hcl file?

A) It stores the current state of managed infrastructure resources so Terraform can detect drift.
B) It records the exact provider versions and package checksums selected during init, ensuring consistent provider installs across team members and CI runs.
C) It defines the minimum required Terraform CLI version for the configuration.
D) It stores sensitive variable values so they are not exposed in plan output.

---

**Q11.** A configuration must create EC2 instances in both us-east-1 and eu-west-1 using the same AWS provider plugin. What is the correct way to configure this in Terraform?

A) Declare two separate provider "aws" blocks that both use the same alias attribute.
B) Declare a single provider "aws" block and pass the region as a per-resource variable inside each resource block.
C) Declare a second provider "aws" block with an alias attribute set to a unique name, then reference it on specific resources using provider = aws.alias_name.
D) List both regions under the required_providers block's version constraint for the aws provider.

---

**Q12.** Which statement best describes the primary purpose of Terraform state?

A) It maps real-world resources to your configuration, records resource metadata, and improves performance when managing large amounts of infrastructure.
B) It stores the HCL source code so configurations can be reused across unrelated projects.
C) It locks provider plugin versions and checksums during terraform init.
D) It generates a human-readable audit log of every API call Terraform makes to the provider.

---

**Q13.** Which block correctly declares a required provider named aws with a specific source and version constraint?

A) terraform {
  required_providers {
    aws = "hashicorp/aws"
    version = "~> 5.0"
  }
}
B) provider "aws" {
  required_providers {
    source = "hashicorp/aws"
    version = "~> 5.0"
  }
}
C) terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
D) required_providers {
  aws = {
    source  = "hashicorp/aws"
    version = "~> 5.0"
  }
}

---

**Q14.** A repository has a committed .terraform.lock.hcl that locks the aws provider to version 5.14.0. A teammate changes required_providers to version = ">= 5.20" and pushes the change, but does not update the lock file. What happens when another engineer runs terraform init on a clean checkout?

A) Terraform automatically rewrites the lock file to the newest matching version without any warning or extra flags.
B) Terraform ignores the lock file entirely and always installs the newest available provider release from the registry.
C) Terraform pauses and interactively prompts the user to approve installing a newer provider version.
D) terraform init fails with an error because the locked version 5.14.0 no longer satisfies the constraint, requiring terraform init -upgrade to resolve it.

---

**Q15.** A DevOps engineer runs `terraform plan` and sees a resource listed as: `-/+ resource "aws_instance" "web" will be replaced, as requested`. What does the `-/+` symbol combination indicate?

A) The resource will be updated in place with zero downtime
B) Terraform detected drift and will only refresh the state file
C) The plan failed and no changes will be applied
D) The resource will be destroyed and then a new resource will be created to replace it

---

**Q16.** Which command checks that a Terraform configuration is syntactically valid and internally consistent, without contacting any remote APIs or backends?

A) terraform validate
B) terraform init
C) terraform apply -auto-approve
D) terraform plan

---

**Q17.** A team's configuration specifies a provider version constraint of `~> 5.0`, and a newer 5.x version has been released. Which command should they run to update the installed provider plugin to the latest version allowed by the constraint?

A) terraform apply -refresh-only
B) terraform init -upgrade
C) terraform providers schema
D) terraform plan -refresh-only

---

**Q18.** An engineer changes the `availability_zone` argument on an existing `aws_instance` resource already tracked in state. Running `terraform plan` shows the instance will be destroyed and recreated instead of updated. What is the most likely explanation?

A) The state file has become corrupted and must be manually repaired
B) Terraform always destroys and recreates any resource that has any argument changed
C) The AWS provider version installed is incompatible with the configuration
D) availability_zone is a ForceNew attribute for this resource type, so the provider requires replacement to apply the change

---

**Q19.** What is the primary purpose of the resource dependency graph that Terraform builds internally during plan and apply?

A) To encrypt sensitive values before writing them to the state file
B) To determine the correct order of create, update, and destroy operations and enable safe parallel execution
C) To automatically generate module documentation from resource definitions
D) To validate that HCL syntax follows canonical formatting conventions

---

**Q20.** A developer wants to destroy only a single misbehaving resource, `aws_instance.web`, so it will be recreated fresh on the next apply, without touching any other resource in the configuration. Which command should they run?

A) terraform taint aws_instance.web
B) terraform state rm aws_instance.web
C) terraform plan -destroy
D) terraform destroy -target=aws_instance.web

---

**Q21.** A developer runs `terraform plan` and sees the following in the output:

  # aws_instance.web must be replaced
-/+ resource "aws_instance" "web" {
      ~ availability_zone = "us-east-1a" -> (known after apply)
      ~ id                 = "i-0123456789" -> (known after apply)
      ~ subnet_id          = "subnet-abc123" -> "subnet-def456" # forces replacement
    }

What is the BEST explanation for why Terraform is proposing to destroy and recreate this resource instead of updating it in place?

A) Terraform always destroys and recreates aws_instance resources when any attribute in the configuration changes
B) The state file is corrupted and Terraform cannot reconcile the drift, so it defaults to recreating the resource
C) The subnet_id argument is defined in the provider schema as ForceNew, meaning it cannot be updated on an existing instance and requires replacement
D) The developer forgot to run terraform validate before terraform plan, causing Terraform to treat the change as invalid

---

**Q22.** A Terraform configuration includes this variable block:

variable "instance_count" {
  type = number
}

The team wants to enforce that instance_count is always greater than 0, and display a custom error message if it is not, without using a separate provider or external tool. Which block should be added inside the variable block to satisfy this requirement?

A) A validation block containing condition = var.instance_count > 0 and error_message = "Instance count must be greater than 0."
B) A lifecycle block containing precondition = var.instance_count > 0
C) A check block containing assert = var.instance_count > 0
D) A validation block containing condition = var.instance_count > 0 and message = "Instance count must be greater than 0."

---

**Q23.** A module needs to create one aws_iam_user resource for each name in a list that may be reordered or have items removed from the middle over time. The team wants removing a single name from the list to destroy only that user's resource, without forcing Terraform to destroy and recreate other users due to index shifting. Which approach should be used?

A) Use count = length(var.user_names) and index into var.user_names[count.index]
B) Use count with a fixed number greater than the list length and pad the list with empty strings
C) Use for_each = var.user_names directly without converting the list to a set or map
D) Convert the list to a set with toset(var.user_names) and use for_each = toset(var.user_names)

---

**Q24.** An aws_security_group resource needs a variable number of ingress rules generated from a list variable called var.ingress_rules, where each element is an object with from_port, to_port, and cidr_blocks. Which HCL construct correctly generates repeated nested ingress blocks from this list?

A) A for expression assigned directly to the ingress argument: ingress = [for r in var.ingress_rules : r]
B) A count argument set on the security group resource itself with count = length(var.ingress_rules)
C) A dynamic block named "ingress_rules" with for_each = var.ingress_rules, since the block name must match the variable name
D) A dynamic "ingress" block with for_each = var.ingress_rules and a content block referencing ingress.value.from_port, ingress.value.to_port, and ingress.value.cidr_blocks

---

**Q25.** A resource block for an aws_instance includes a lifecycle block with a precondition that checks self.ami is not empty, and the condition evaluates to false during terraform apply. What is the correct description of Terraform's behavior?

A) Terraform halts execution and reports an error referencing the precondition's error_message before the resource is created or updated
B) Terraform logs a warning but continues applying the resource, since preconditions are advisory only
C) Terraform silently skips creating that resource but continues applying all other resources in the configuration
D) Terraform automatically substitutes a default AMI value and continues the apply

---

**Q26.** An output value in a root module is defined as:

output "db_password" {
  value     = var.db_password
  sensitive = true
}

Which statement correctly describes the effect of setting sensitive = true on this output?

A) Terraform CLI redacts the value from plan and apply console output, but the value is still stored in plain text in the state file
B) The value is encrypted before being written to the state file, so it cannot be read even with access to the state
C) The output is excluded entirely from the state file, so terraform output cannot retrieve it under any circumstances
D) Terraform automatically integrates with HashiCorp Vault to store the value externally instead of in state

---

**Q27.** A configuration creates an aws_iam_policy resource and a separate aws_instance resource whose user_data script calls an internal API that requires the IAM policy to already exist, but the aws_instance resource's arguments do not reference any attribute of the aws_iam_policy resource. Which construct ensures Terraform creates the IAM policy before the EC2 instance, given there is no implicit attribute reference to create a dependency?

A) Add depends_on = [aws_iam_policy.example] inside the aws_instance resource block
B) Add a precondition block on the aws_instance resource checking that the policy exists
C) Rename the aws_instance resource so it alphabetically sorts after aws_iam_policy in the configuration file
D) Add a provisioner "local-exec" block that sleeps for 30 seconds before instance creation

---

**Q28.** A data source block in your configuration reads an AWS AMI and must fail the plan with a custom error message if the returned AMI is not tagged "encrypted = true". Which construct should you add inside the data block to enforce this?

A) A variable "validation" block referencing the data source's attribute
B) A "lifecycle" block containing a "precondition"
C) A "lifecycle" block containing a "postcondition"
D) A separate "check" block that references a null_resource

---

**Q29.** A team wants to reuse a VPC module from the public Terraform Registry. They need to automatically receive patch-level bug fixes but must NOT automatically receive minor or major version changes. Which version constraint achieves this?

A) version = "~> 3.0"
B) version = ">= 3.0"
C) version = "= 3.0.0"
D) version = "<= 3.0"

---

**Q30.** A platform team maintains a network module in a private GitHub repository that is only accessible over SSH (no HTTPS credentials configured). Which module source correctly references this module?

A) source = "registry.terraform.io/org/network-module/aws"
B) source = "github.com/org/network-module"
C) source = "git::ssh://git@github.com/org/network-module.git"
D) source = "./modules/network-module"

---

**Q31.** Which statement correctly describes the version argument inside a module block?

A) It is only supported for modules sourced from a Terraform Registry (public or private).
B) It must always specify an exact version number with no ranges.
C) It is required in every module block regardless of the source type.
D) It can be used with any module source, including local paths and generic Git URLs.

---

**Q32.** A child module named 'storage' declares an output block named 'bucket_arn'. What is the correct syntax for the root module to reference this value elsewhere in its configuration?

A) output.storage.bucket_arn
B) data.storage.bucket_arn
C) module.storage.bucket_arn
D) var.storage.bucket_arn

---

**Q33.** A root module calls a 'network' module that outputs 'environment_tier', and a 'database' module that requires an input variable 'instance_class' set to that value. Which module block correctly wires this dependency?

A) module "database" {
  source = "./modules/database"
  instance_class = var.network.environment_tier
}
B) module "network" {
  source = "./modules/database"
  instance_class = network.environment_tier
}
C) module "database" {
  source = "./modules/database"
  instance_class = data.network.environment_tier
}
D) module "database" {
  source = "./modules/database"
  instance_class = module.network.environment_tier
}

---

**Q34.** A team of five engineers all run `terraform apply` from their local machines against the same infrastructure using the default local backend. They frequently encounter corrupted state and conflicting changes when two people apply around the same time. Which change BEST resolves this?

A) Add a `lifecycle { prevent_destroy = true }` block to every resource
B) Have each engineer run `terraform refresh` before every apply
C) Share a single `terraform.tfvars` file via email before each apply
D) Migrate to a remote backend that supports state locking, such as HCP Terraform or S3 with native locking

---

**Q35.** A resource block manages a database whose `admin_password` attribute is marked as `sensitive = true` in the Terraform configuration. A security reviewer asks whether the password is safe from exposure. Which statement is correct?

A) Marking the attribute sensitive prevents Terraform from ever writing it into the state file
B) Terraform automatically encrypts all sensitive values inside the local state file by default
C) The state file may still contain the password in plaintext; access must be restricted using an encrypted remote backend with strict IAM/permissions and locking
D) Using the local backend instead of a remote one guarantees the password cannot be exposed

---

**Q36.** Which backend option provides remote state storage AND state locking natively, without requiring the operator to provision or configure a separate locking mechanism?

A) Local backend
B) HCP Terraform (Terraform Cloud) remote backend
C) S3 backend configured only with a bucket and no additional locking table or feature enabled
D) An `http` backend implementation that does not implement lock/unlock endpoints

---

**Q37.** An engineer needs to stop Terraform from managing a specific resource going forward, but must NOT destroy the actual infrastructure object it represents. Which command should be used?

A) terraform destroy -target=<resource>
B) terraform taint <resource>
C) terraform state rm <resource>
D) terraform import <resource> <id>

---

**Q38.** A module was refactored so that `aws_instance.web` is now written as `aws_instance.app_server`. To avoid Terraform planning to destroy the old resource and create a new one, which configuration block should be added to the module?

A) A `removed` block referencing aws_instance.web
B) A `lifecycle { create_before_destroy = true }` block on aws_instance.app_server
C) A `moved` block with `from = aws_instance.web` and `to = aws_instance.app_server`
D) An `import` block targeting the existing instance ID

---

**Q39.** What is the primary purpose of the Terraform state file?

A) It maps configuration-defined resources to real-world infrastructure objects and tracks resource metadata and dependencies
B) It stores a human-readable copy of the .tf configuration files for documentation purposes
C) It stores the provider plugin binaries used during apply
D) It permanently logs the full history of every past apply and plan output

---

**Q40.** A platform team manages 40 HCP Terraform workspaces that all need the same AWS credentials and a shared 'environment=prod' variable, but they don't want to copy-paste these variables into every workspace individually. Which HCP Terraform feature BEST meets this requirement?

A) Create a module with default variables and require every workspace to call it
B) Define a variable set containing the shared variables and apply it to the relevant workspaces
C) Use a run trigger to pass variables between workspaces
D) Store the variables in a shared remote state data source

---

**Q41.** Which policy-as-code framework is natively integrated into HCP Terraform for enforcing governance rules on runs, distinct from the Open Policy Agent (OPA) integration also supported?

A) Sentinel
B) AWS Config
C) Checkov
D) Terraform Cloud Agent

---

**Q42.** A team wants HCP Terraform runs to authenticate to AWS during provisioning without storing long-lived static access keys as workspace variables. Which HCP Terraform feature should they configure?

A) Variable sets with sensitive variables
B) Run triggers
C) Dynamic provider credentials using OIDC
D) Sentinel policy

---

**Q43.** A platform team has a 'networking' workspace and an 'app' workspace. They want every successful apply in networking to automatically queue a new run in app. Which HCP Terraform feature enables this?

A) Sentinel policy
B) Variable sets
C) Run triggers
D) Workspace tags

---

**Q44.** What does HCP Terraform's drift detection feature do?

A) Automatically applies changes to fix drift without approval
B) Rolls back state to a previous version when drift is detected
C) Locks the workspace to prevent any further runs
D) Periodically runs a plan against workspaces (without applying) to detect changes made outside Terraform

---

