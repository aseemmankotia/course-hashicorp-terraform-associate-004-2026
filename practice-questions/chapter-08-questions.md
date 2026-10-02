## Chapter 8: Interact with Terraform Modules — Practice Questions

**Q1.** A configuration includes the following module block:

module "vpc" {
  source  = "./modules/vpc"
  version = "~> 3.0"
}

What happens when you run terraform init?

A) Terraform downloads version 3.x of the module from the local filesystem
B) Terraform ignores the version argument silently and initializes successfully
C) Terraform treats the local path as a registry alias and queries the public registry
D) Terraform init fails with an error because version is not a valid argument for local path sources

<details>
<summary>Answer</summary>

**Correct: D**

The version argument is only supported for modules sourced from a module registry (public or private). Using it with a local path source produces an error during terraform init/validate.

**Why the others are wrong:**
- Local paths have no version concept — there's nothing to 'download' since the code is already on disk
- Terraform does not silently ignore invalid arguments; it raises a configuration error
- Local paths starting with ./ or ../ are never reinterpreted as registry addresses

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** You need to pass the VPC ID created inside a child module to a resource defined in the root module. The child module declares:

output "vpc_id" {
  value = aws_vpc.this.id
}

and is instantiated as module "network" { source = "./modules/network" }. Which expression correctly references this value in the root module?

A) aws_vpc.this.id
B) module.network.vpc_id
C) network.vpc_id
D) module.network.output.vpc_id

<details>
<summary>Answer</summary>

**Correct: B**

Module outputs are referenced from the calling configuration using module.<MODULE_NAME>.<OUTPUT_NAME>, where MODULE_NAME matches the label given in the module block.

**Why the others are wrong:**
- aws_vpc.this.id only resolves inside the child module's own scope — the root module cannot see child resources directly
- Referencing the module name without the 'module.' prefix is invalid syntax
- There is no intermediate 'output' keyword in the reference chain

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A company hosts internal Terraform modules on Terraform Enterprise at the hostname app.terraform.io under the organization 'example-corp'. Which source argument correctly references the 'vpc' module for AWS from this private registry?

A) source = "example-corp/vpc/aws"
B) source = "git::https://app.terraform.io/example-corp/vpc-aws.git"
C) source = "private-registry.terraform.io/example-corp/vpc/aws"
D) source = "app.terraform.io/example-corp/vpc/aws"

<details>
<summary>Answer</summary>

**Correct: D**

Private registry module sources use the four-part format <HOSTNAME>/<NAMESPACE>/<NAME>/<PROVIDER>. Omitting the hostname defaults to the public Terraform Registry (registry.terraform.io), so the hostname must be explicit for a private registry.

**Why the others are wrong:**
- This three-part format resolves against the public registry.terraform.io, not the private one
- Git-style sourcing bypasses the registry protocol entirely and loses version constraint support tied to registry metadata
- This is not the correct registry hostname convention — the private registry runs at whatever custom hostname is configured, here app.terraform.io

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which source argument correctly retrieves a module from a specific tag in a GitHub repository using the generic Git source protocol?

A) source = "git::https://github.com/hashicorp/example.git?ref=v1.2.0"
B) source = "github.com/hashicorp/example?ref=v1.2.0"
C) source = "hashicorp/example/aws" with version = "v1.2.0"
D) source = "https://github.com/hashicorp/example//v1.2.0"

<details>
<summary>Answer</summary>

**Correct: A**

The generic Git source format is git::<repo-url>, and a specific ref (branch, tag, or commit) is appended with ?ref=<value>. This is the syntax the exam expects for Git-hosted modules outside the registry protocol.

**Why the others are wrong:**
- github.com/... shorthand works without git:: prefix but the exam distinguishes it as a separate shorthand, not the 'generic Git' form being tested here — also this option is technically valid but option B is the canonical documented generic form being asked for
- The version argument only applies to registry modules, not Git sources — Git modules use ?ref= instead
- Appending a path segment like //v1.2.0 is used for subdirectories, not for selecting a ref/tag

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A module is sourced from the public Terraform Registry using source = "terraform-aws-modules/vpc/aws". Which format rule must this string follow?

A) <PROVIDER>/<NAME>/<NAMESPACE>
B) <NAME>/<NAMESPACE>/<PROVIDER>
C) <NAMESPACE>/<NAME>/<PROVIDER>
D) <PROVIDER>/<NAMESPACE>/<NAME>

<details>
<summary>Answer</summary>

**Correct: C**

Public registry module addresses always follow NAMESPACE/NAME/PROVIDER — here 'terraform-aws-modules' is the namespace, 'vpc' is the name, and 'aws' is the target provider.

**Why the others are wrong:**
- This reverses namespace and provider position
- This puts the name before the namespace, which is incorrect ordering
- Provider must be the last segment, not the first

</details>

---

**Q6.** A module block specifies version = "~> 1.2". Which of the following versions satisfies this constraint?

A) 1.1.9
B) 2.0.0
C) 1.3.5
D) 1.2.0-beta

<details>
<summary>Answer</summary>

**Correct: C**

With only two version components specified, ~> 1.2 allows any version >= 1.2.0 and < 2.0.0, meaning the minor and patch numbers can increase but the major version cannot. 1.3.5 satisfies this range.

**Why the others are wrong:**
- 1.1.9 is below the 1.2.0 floor and does not satisfy the constraint
- 2.0.0 changes the major version, which the ~> constraint explicitly locks
- Pre-release versions like 1.2.0-beta are excluded by default unless explicitly matched

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A child module declares a variable named 'environment' but no value is set for it in the module block, and the variable has no default. What happens during terraform plan?

A) Terraform inherits the value from a root module variable of the same name automatically
B) The variable defaults to an empty string silently
C) Terraform plan fails, prompting for the missing required variable or erroring in non-interactive mode
D) Terraform searches parent scopes for a matching variable name before failing

<details>
<summary>Answer</summary>

**Correct: C**

Module variable scope is fully isolated — variables are never implicitly inherited by name. A required variable with no default and no explicit assignment in the module block causes Terraform to prompt for it interactively or fail in automation.

**Why the others are wrong:**
- Variable scope isolation means there is no automatic name-based inheritance between root and child modules
- Terraform does not silently default undeclared-default variables to empty string
- There is no scope-searching behavior across module boundaries — every value must be explicitly passed

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the correct conceptual description of Terraform module variable scope?

A) Each module has its own isolated variable namespace; values must be explicitly passed in via the module block's arguments
B) All variables declared anywhere in the configuration are globally accessible to every module
C) Root module variables automatically override any variable of the same name in child modules
D) Child modules can directly reference root module resource attributes without passing them as variables

<details>
<summary>Answer</summary>

**Correct: A**

Terraform enforces strict variable scope isolation per module. A child module cannot see the root module's variables, resources, or locals unless they are explicitly passed in as arguments in the module block, and its outputs must be explicitly referenced back via module.<name>.<output>.

**Why the others are wrong:**
- There is no global variable namespace shared across modules — this would break module reusability
- Same-named variables in different modules are entirely independent; there is no override behavior
- Direct resource attribute access across module boundaries is not possible; only declared outputs are exposed

</details>

---

