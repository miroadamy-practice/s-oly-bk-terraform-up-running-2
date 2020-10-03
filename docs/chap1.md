# Why Terraform

Task - setting up webserver. There are many ways how to set it up

## Ad-Hoc scripts

``` bash
# Update the apt-get cache
sudo apt-get update

# Install PHP and Apache
sudo apt-get install -y php apache2

# Copy the code from the repository
sudo git clone https://github.com/brikis98/php-app.git /var/www/html/app

# Start Apache
sudo service apache2 start
```

See [code](https://github.com/brikis98/terraform-up-and-running-code/tree/master/code/bash/01-why-terraform)

Problem: 

* will become unmaintanable
* not declarative (run twice, installs twice)

## Configuration tools

E.g. Ansible, Chef, Puppet, SaltStack

Ansible role `webserver.yaml`:

``` yaml
{% include 'src/code/ansible/01-why-terraform/webserver.yml' %}
```

How it is better than Bash:

* idempotent (can run twice)
* works on remote: create `hosts` file, runs on them

``` bash
{% include 'src/code/ansible/01-why-terraform/hosts' %}
```

Define playbook `playbook.yaml`:

``` yaml
{% include 'src/code/ansible/01-why-terraform/playbook.yml' %}
```

and run

``` bash
ansible-playbook playbook.yml
```

See [code](https://github.com/brikis98/terraform-up-and-running-code/tree/master/code/ansible/01-why-terraform)

## Server templating tool

E.g. Packer, Docker, Vagrant

Instead of launching a bunch of servers and configuring them by running the same code on each one, the idea behind server templating tools is to create an image of a server that captures a fully self-contained “snapshot” of the operating system (OS), the software, the files, and all other relevant details. You can then use some other IaC tool to install that image on all of your servers


Tools working with images are operating on:

* virtual machines - Packer, Vagrant. VM image runs on top of hypervisor, virtualizes hardware 
* containers - Docker, CoreOs rkt - isolates processes, memory, mount points, and networking, shares kernel and HW

### Example

Packer script => AMI

``` yaml
{% include 'src/code/packer/01-why-terraform/webserver.json' %}
```

Run 

```
packer build webserver.json
```

=> this is the path to immutable infrastructure: once you’ve deployed a server, you never make changes to it again. If you need to update something, such as deploy a new version of your code, you create a new image from your server template and you deploy it on a new server. Because servers never change, it’s a lot easier to reason about what’s deployed.

## Orchestration tools

Take care of deployment of the images at scale, monitoring, scaling, traffic distribution

E.g. Kubernetes, Marathon/Mesos, Amazon ECS, Docker Swarm, and Nomad 

The Webserver deployed to K8s cluster:

```
apiVersion: apps/v1

# Use a Deployment to deploy multiple replicas of your Docker
# container(s) and to declaratively roll out updates to them
kind: Deployment

# Metadata about this Deployment, including its name
metadata:
  name: example-app

# The specification that configures this Deployment
spec:
  # This tells the Deployment how to find your container(s)
  selector:
    matchLabels:
      app: example-app

  # This tells the Deployment to run three replicas of your
  # Docker container(s)
  replicas: 3

  # Specifies how to update the Deployment. Here, we
  # configure a rolling update.
  strategy:
    rollingUpdate:
      maxSurge: 3
      maxUnavailable: 0
    type: RollingUpdate

  # This is the template for what container(s) to deploy
  template:

    # The metadata for these container(s), including labels
    metadata:
      labels:
        app: example-app

    # The specification for your container(s)
    spec:
      containers:

        # Run Apache listening on port 80
        - name: example-app
          image: httpd:2.4.39
          ports:
            - containerPort: 80
```

Use `kubectl apply -f example-app.yml`

## Provisioning Tools

Whereas configuration management, server templating, and orchestration tools define the code that runs on each server, provisioning tools such as Terraform, CloudFormation, and OpenStack Heat are responsible for creating the servers themselves.

You can use provisioning tools to not only create servers, but also databases, caches, load balancers, queues, monitoring, subnet configurations, firewall settings, routing rules, Secure Sockets Layer (SSL) certificates, and almost every other aspect of your infrastructure

Provisioning Web server with TF:

``` tf
resource "aws_instance" "app" {
  instance_type     = "t2.micro"
  availability_zone = "us-east-2a"
  ami               = "ami-0c55b159cbfafe1f0"

  user_data = <<-EOF
              #!/bin/bash
              sudo service apache2 start
              EOF
}
```

This is path to IaaC - infrastructure as a code.

### Benefits of IaaC:

* Self-service - no sysadmin bottleneck
* Speed and safety - automation
* Documentation 
* Version control
* Validation
* Reuse
* Happiness

## How terraform compares

Other major tools:
 

### Configuration Management Versus Provisioning

* Chef, Puppet, Ansible, and SaltStack are all configuration management tools, whereas CloudFormation, Terraform, and OpenStack Heat are all provisioning tools
* can work in tandem: Terraform + config management

### Mutable Infrastructure Versus Immutable Infrastructure

* natural for provisioning tools
* procedural tools need to know state and order => full history of all changes

### Master vs Masterless
    
* Chef, Puppet, and SaltStack all require that you run a master server
    * needs extra infrastructure
    * needs maintenance
    * security
* Ansible, CloudFormation, Heat, and Terraform are all masterless by default

### Agent Versus Agentless

* Chef, Puppet, and SaltStack all require you to install agent software on each target
    * bootstrapping
    * needs maintenance
    * security
* Ansible, CloudFormation, Heat, and Terraform do not require you to install any extra agents

### community and ecosystem

* Terraform and Ansible wins

### Combinations

* Provisioning plus configuration management => TF + Ansible
* Provisioning plus server templating - TF + Packer (builds images)
* Provisioning plus server templating plus orchestration - TF + Packer + Docker

