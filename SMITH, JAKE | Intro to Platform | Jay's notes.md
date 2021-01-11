# Notes on Jake Smith, "Intro to Platform"

video link: https://drive.google.com/file/d/14Ir4UufgVEh31-qXmcbRPDJir9v21uur/view

Platform is a series of virtual machines.

### Terraform (a Hashicorp tool)
: tool for configuring resources
* stateful tool (stateful configuration, set by Omada's Tah-Ruk app, determines the nature of our resources)
* used for allocation of resources
* Examples of things it can configure:
  * db instances
  * S3 buckets
  * SQS & SNS queues
  * Redis servers
  * Elasticsearch servers

  * network & DNS configuration
  * AWS instances (via the Hashicorp tool Packer)
  * storage (e.g., S3, disk volumes databases)
  * RDS (relational database service)
  * Identity & Access Management (IAM) / permissioning
  * SNS/SQS
  * Redis
  * Elasticsearch

- Tah-Ruk is our repo use to configure Terraform

### Ansible (owned by Red Hat, now an IBM subsidiary)
: tool for configuring resources when they are already running
* stateless tool (doesn't know how things get to their final states)
* used when a server is running
* These days we use this only for
  * database permissions (users, permissions, grants)
  * platform's other secret store

### Vault (a Hashicorp tool)
: controlled secrets access
* fine-grained permissions

### Each server we manage (Omada Virtual Private Cloud (VPC))
1. Prometheus stack for monitoring metrics
1. Vault & Consul
1. Nomad
-- should be same versions of everything so that we can deploy the same code to each one
  implications
  * consistency (can deploy the same code to each one)
  * complicance: helps us ensure there is no PHI outside of production environment
  * contained

### Bonus Infra Servers
* Goanywhere
* Jenkins (3) + Workers
* IT/Security Machines & more

### Docker: standalone operating system
: alllows us to run a standard image everywhere
As a concept, Docker containers are stateless. They do not write to disk, because as soon as the container goes away, the data goes away.
However, they can in practice store state by calling to a connected database, elasticsearch, S3, etc.

### Nomad
: essentially just a scheduler
with some guarantees:
1. If you give Nomad a job, it will be scheduled if at all possible (e.g., so long as the job spec is fine and your container somewhat works)
1. Memory and CPU:
  * if your container exceeds the amount of memory allocated, it will be killed.
  * you will get at least as many CPU units as you ask for.
1. A container will receive traffic only if it is passing health checks.

### Consul
: Nomad uses this to register which apps are living at which IP / port combinations
* this is what routes users to one Nomad container or another.
  * As of Jake's talk, we were basically just randomly routing users to one nomad container or another
  * The plan as of then was to upgrade to L7 Routing

### Cloudflare handles our DNS

### PLATFORM ALSO MAINTAINS
* The monitoring stack
  * Prometheus: metrics, alerting, graphs

### MY QUESTIONS
-> What should I know about Nomad?
-> What should I know about AWS?
