## Chapter 6: Writing HCL: Resources, Data Sources & Dependencies — Practice Questions

**Q1.** An aws_instance uses an IAM instance profile whose name is passed in via a variable, not derived from an aws_iam_role_policy_attachment resource's attributes. Terraform applies the instance before the policy attachment finishes, causing the app to fail on first boot. Which fix ensures correct ordering?

A) Replace count with for_each on the aws_instance resource
B) Run terraform apply -target to force ordering permanently
C) Add depends_on = [aws_iam_role_policy_attachment.example] to the aws_instance resource
D) Add a null_resource with a local-exec provisioner inside the policy attachment

<details>
<summary>Answer</summary>

**Correct: C**

There is no attribute reference linking the instance to the policy attachment, so Terraform can't infer the dependency. depends_on explicitly forces the policy attachment to complete first.

**Why the others are wrong:**
- count/for_each control resource multiplicity, not ordering between unrelated resources
- -target is a manual one-time CLI override, not a configuration-level dependency fix
- A null_resource provisioner adds complexity and doesn't guarantee ordering relative to the instance resource

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An S3 bucket named 'prod-logs' was created manually in the AWS console. You now want Terraform to fully own its lifecycle (create, update, destroy going forward) as part of your codebase. What should you do?

A) Write a resource "aws_s3_bucket" "prod_logs" block matching its config, then run terraform import to bring it into state
B) Write a data "aws_s3_bucket" "prod_logs" block referencing it
C) Add depends_on pointing at a data source for the bucket
D) Reference the bucket name through a variable and skip Terraform management

<details>
<summary>Answer</summary>

**Correct: A**

A data block is read-only and never grants Terraform ownership. To manage lifecycle, you need a resource block plus terraform import to associate it with existing state.

**Why the others are wrong:**
- data blocks can never create, update, or destroy the underlying infrastructure
- depends_on doesn't grant ownership; it only affects ordering between resources it's attached to
- Using only a variable means Terraform has no configuration for the bucket at all, so it can't manage it

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** You need the latest Amazon Linux 2023 AMI ID at every apply, without hardcoding an AMI ID that changes over time, for use in an aws_instance's ami argument. Which approach is correct?

A) Use a data "aws_ami" "latest" block with owners and filter arguments, then reference data.aws_ami.latest.id
B) Define variable "ami_id" with a default value updated manually each release
C) Create an output "ami_id" block and hardcode the value inside it
D) Use resource "aws_ami_copy" to generate a new AMI on every apply

<details>
<summary>Answer</summary>

**Correct: A**

A data source dynamically looks up the current matching AMI at plan/apply time, avoiding manual hardcoding.

**Why the others are wrong:**
- A variable default still requires manual updates and doesn't reflect real-time AWS state
- Outputs only expose values after apply; they don't perform lookups and shouldn't hardcode dynamic values
- aws_ami_copy manages AMI copies, not queries for existing public AMIs, and would create unwanted resources

</details>

---

**Q4.** You need an input variable 'environment' that must be restricted to exactly 'dev', 'staging', or 'prod', failing terraform plan with a clear message otherwise. Which variable configuration achieves this?

A) type = string with no other arguments
B) type = string with a validation block containing a condition and error_message
C) default = "dev" with no type specified
D) sensitive = true on the variable block

<details>
<summary>Answer</summary>

**Correct: B**

The validation block is the only mechanism that enforces custom constraints on allowed values and produces a plan-time error message.

**Why the others are wrong:**
- type = string alone allows any string value, no restriction on allowed set
- A default value doesn't restrict what callers can pass in
- sensitive only hides output display; it has nothing to do with value restriction

</details>

---

**Q5.** Which output block correctly ensures a generated database password is never displayed in CLI output during plan or apply?

A) output "db_password" { value = aws_db_instance.main.password\n sensitive = "true" }
B) variable "db_password" { value = aws_db_instance.main.password\n sensitive = true }
C) output "db_password" { value = aws_db_instance.main.password\n sensitive = true }
D) output "db_password" { sensitive = true\n aws_db_instance.main.password }

<details>
<summary>Answer</summary>

**Correct: C**

sensitive is a boolean argument (true, not a quoted string) inside an output block, alongside value assigning the attribute reference.

**Why the others are wrong:**
- sensitive must be a boolean literal true, not the string "true"
- variable blocks don't have a value argument for computed resource attributes and don't suppress console output the same way
- Missing the required value argument entirely — syntax is invalid HCL

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Which is the correct, valid depends_on syntax inside a resource block?

A) depends_on = "aws_iam_role.example"
B) depends_on = aws_iam_role.example
C) depends_on = [aws_iam_role.example.id]
D) depends_on = [aws_iam_role.example]

<details>
<summary>Answer</summary>

**Correct: D**

depends_on requires a list of resource or module references (not attributes), e.g. depends_on = [aws_iam_role.example].

**Why the others are wrong:**
- It must be a list, not a plain string
- It must be wrapped in a list (square brackets), a bare reference is invalid
- depends_on expects resource/module references, not specific attributes like .id — this is a common exam trap

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** An aws_instance's subnet_id argument references aws_subnet.main.id. You run terraform destroy on the whole configuration. In what order are these two resources destroyed?

A) Random order — destroy does not consider dependency graphs
B) aws_subnet.main is destroyed first, then aws_instance
C) aws_instance is destroyed first, then aws_subnet.main
D) Both are always destroyed in parallel regardless of references

<details>
<summary>Answer</summary>

**Correct: C**

Terraform destroys resources in reverse dependency order — the dependent (aws_instance) is removed before the dependency (aws_subnet) it references.

**Why the others are wrong:**
- Terraform builds a dependency graph and destroy respects it; it is not random
- Destroying the subnet first would leave the instance referencing a deleted resource, violating the dependency order
- Parallelism only applies to independent resources, not ones linked by a reference

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the core functional difference between a resource block and a data block in Terraform?

A) resource blocks are read-only; data blocks create infrastructure
B) data blocks require their own provider configuration separate from resources
C) resource blocks manage the full lifecycle (create, update, destroy) of infrastructure; data blocks only read existing infrastructure without managing it
D) resource blocks only work with cloud providers; data blocks work with any provider type

<details>
<summary>Answer</summary>

**Correct: C**

This is the fundamental distinction tested repeatedly: resource blocks are the unit of management (CRUD), data blocks are read-only lookups against existing infrastructure or external systems.

**Why the others are wrong:**
- This reverses the actual roles of resource and data blocks
- Both block types use the provider already configured in the same configuration; no separate provider config is required
- Both resource and data blocks are provider-agnostic constructs; the distinction isn't about provider type

</details>

---

