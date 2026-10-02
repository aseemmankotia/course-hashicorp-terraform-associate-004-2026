## Chapter 8: Interact with Terraform Modules — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Public Terraform Registry module source format? | <NAMESPACE>/<NAME>/<PROVIDER> e.g. terraform-aws-modules/vpc/aws. Exam tip: exactly three slash-separated parts, no hostname = defaults to registry.terraform.io |
| 2 | Private registry module source format? | <HOSTNAME>/<NAMESPACE>/<NAME>/<PROVIDER> e.g. app.terraform.io/example-corp/vpc/aws. Exam tip: four parts, hostname is required to escape the public registry default |
| 3 | Local module source path format? | Must begin with ./ or ../ e.g. source = "./modules/vpc". Exam tip: version argument is NOT allowed with local sources — causes an error |
| 4 | Generic Git module source syntax? | source = "git::https://example.com/repo.git?ref=v1.0.0". Exam tip: use ?ref= to pin branch/tag/commit; the 'version' argument does not apply to Git sources |
| 5 | Does the 'version' argument work on local path module sources? | No. Version constraints only apply to Registry module sources (public or private). Local and Git sources will error or ignore version pinning via that argument. |
| 6 | Version constraint ~> 1.2 meaning? | Allows >= 1.2.0 and < 2.0.0 (minor/patch can increment, major cannot). Exam tip: two-part ~> pins the major version only. |
| 7 | Version constraint ~> 1.2.0 meaning? | Allows >= 1.2.0 and < 1.3.0 (only patch can increment). Exam tip: three-part ~> pins major AND minor version. |
| 8 | Syntax to reference a module's output in the root module? | module.<MODULE_NAME>.<OUTPUT_NAME> e.g. module.network.vpc_id. Exam tip: MODULE_NAME is the label given after 'module' in the block, not the source path. |
| 9 | How do you pass a variable value into a module? | Set it as an argument inside the module block, matching a declared variable name in the module: module "vpc" { source = "./vpc"; cidr_block = "10.0.0.0/16" } |
| 10 | Are variables automatically shared between root and child modules? | No. Modules have fully isolated variable scope — nothing is inherited by name. Every value must be explicitly passed in, and every output explicitly referenced back. |
| 11 | Which CLI command downloads/updates modules referenced in configuration? | terraform init (or terraform get for module-only refresh without full init). Exam tip: init also initializes providers and backend, get only handles modules. |
| 12 | Can a module call another module (nested modules)? | Yes. Modules can be composed/nested arbitrarily deep. Exam tip: outputs must be explicitly passed up through each intermediate module — there is no automatic pass-through. |
