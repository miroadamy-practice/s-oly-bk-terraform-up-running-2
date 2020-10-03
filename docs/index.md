# Terraform Up and Running - Second Edition

![](https://learning.oreilly.com/library/cover/9781492046899/)

* by: Yevgenij Brinkman
* Publisher: O'Reilly Media, Inc.
* Release Date: September 2019
* ISBN: 9781492046905

[OReilly book](https://learning.oreilly.com/library/view/terraform-up/9781492046899/)
[Source code - original](https://github.com/brikis98/terraform-up-and-running-code.git)

## TOC


### Chapter 1, [Why Terrraform](chap1.md)
How DevOps is transforming the way we run software; an overview of infrastructure as code tools, including configuration management, server templating, orchestration, and provisioning tools; the benefits of infrastructure as code; a comparison of Terraform, Chef, Puppet, Ansible, SaltStack, OpenStack Heat, and CloudFormation; how to combine tools such as Terraform, Packer, Docker, Ansible, and Kubernetes.

### Chapter 2, [Getting Started with Terraform](chap2.md)
Installing Terraform; an overview of Terraform syntax; an overview of the Terraform CLI tool; how to deploy a single server; how to deploy a web server; how to deploy a cluster of web servers; how to deploy a load balancer; how to clean up resources you’ve created.

### Chapter 3, [How to Manage Terraform State](chap3.md)
What Terraform state is; how to store state so that multiple team members can access it; how to lock state files to prevent race conditions; how to manage secrets with Terraform; how to isolate state files to limit the damage from errors; how to use Terraform workspaces; a best-practices file and folder layout for Terraform projects; how to use read-only state.

### Chapter 4, [How to Create Reusable Infrastructure with Terraform Modules](chap4.md)
What modules are; how to create a basic module; how to make a module configurable with inputs and outputs; local values; versioned modules; module gotchas; using modules to define reusable, configurable pieces of infrastructure.

### Chapter 5, Terraform Tips and Tricks: Loops, If-Statements, Deployment, and Gotchas
Loops with the count parameter, for_each and for expressions, and the for string directive; conditionals with the count parameter, for_each and for expressions, and the if string directive; built-in functions; zero-downtime deployment; common Terraform gotchas and pitfalls, including count and for_each limitations, zero-downtime deployment gotchas, how valid plans can fail, refactoring problems, and eventual consistency.

### Chapter 6, Production-Grade Terraform Code
Why DevOps projects always take longer than you expect; the production-grade infrastructure checklist; how to build Terraform modules for production; small modules; composable modules; testable modules; releasable modules; Terraform Registry; Terraform escape hatches.

### Chapter 7, How to Test Terraform Code
Manual tests for Terraform code; sandbox environments and cleanup; automated tests for Terraform code; Terratest; unit tests; integration tests; end-to-end tests; dependency injection; running tests in parallel; test stages; retries; the test pyramid; static analysis; property checking.

### Chapter 8, How to Use Terraform as a Team
How to adopt Terraform as a team; how to convince your boss; a workflow for deploying application code; a workflow for deploying infrastructure code; version control; the golden rule of Terraform; code reviews; coding guidelines; Terraform style; CI/CD for Terraform; the deployment process.
