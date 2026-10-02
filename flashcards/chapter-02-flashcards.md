## Chapter 2: Terraform vs Other IaC & Configuration Tools — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Terraform's configuration approach: declarative or procedural? | Declarative — you specify desired end state; Terraform computes the steps. Exam tip: procedural tools require ordered step-by-step instructions. |
| 2 | What does `terraform plan` do? | Produces a preview of add/change/destroy actions by diffing current state vs desired config — makes NO changes to real infrastructure. |
| 3 | Flag to save a plan to a file for later apply? | `terraform plan -out=tfplan`, then `terraform apply tfplan` to apply exactly the reviewed plan. |
| 4 | Command to visualize the resource dependency graph? | `terraform graph` — outputs DOT format, render with Graphviz. Shows resource creation ordering. |
| 5 | How does Terraform decide resource creation order? | Builds a directed acyclic resource graph from implicit/explicit references — dependencies are created first, independent resources in parallel. |
| 6 | Terraform vs Ansible: core positioning difference? | Terraform = state-driven provisioning of infrastructure. Ansible = agentless configuration management of software/config on running nodes. Exam tip: they complement, not compete. |
| 7 | Common exam-tested integration pattern for Terraform + Ansible? | Terraform provisions infrastructure (VMs, networks); Ansible then configures the OS/app layer on those provisioned instances. |
| 8 | Agent-based vs agentless config management — example of each? | Agent-based: Chef, Puppet (require installed client/agent). Agentless: Ansible (uses SSH/WinRM, no persistent agent). |
| 9 | Where are Terraform providers and community modules published? | The Terraform Registry (registry.terraform.io) — hosts providers and both community and HashiCorp-verified modules. |
| 10 | What is a Terraform 'provider'? | A plugin that translates HCL resource blocks into API calls for a specific platform (AWS, Azure, GCP, etc.) — the mechanism enabling the multi-cloud provider ecosystem. |
| 11 | remote-exec provisioner — recommended or discouraged? | Discouraged as a last resort by HashiCorp — breaks idempotency/declarative model. Prefer dedicated config management tools (e.g., Ansible) instead. |
| 12 | State-driven provisioning vs procedural scripts — key exam distinction? | Terraform tracks a state file representing real-world resources, enabling drift detection and plan diffing. Procedural scripts have no persistent state model of infrastructure. |
