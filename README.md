# Overview
This repo contains simple kubernetes manifest files to deploy PiHole.

# Prerequisites
* You must have kubernetes deployed along with a load balancer to run this out of the box.

# My Infrastructure
* Microk8s on 3 Raspberry Pi 5s
* Load Balancing with MetalLB by enabling it on microk8s
* CoreDNS must also be enabled on microk8s

# Deployment
## Makefile
* A Makefile is including if you're able to run the make command. It will build the Go script.
    * The Go script only removes passwords from plain text in yaml files.
## Manifest Files
* Deployment file
    * This file contains the password for your pihole's web interface.
* Service file
    * This file contains the ip address for your load balancer.
* Namespace file
## Kubernetes
* From the project's root directory, you should be able to run "kubectl apply -f deploy"
* Apply individual manifest files by changing directory into deploy and running "kubectl apply -f file.yaml"

# Basic Kubectl for Verification
Run the following commands to verify the PiHole app is running
* kubectl get pods -o wide -n pihole
* kubectl get services -o wide -n pihole
    * Make sure the EXTERNAL-IP here matches the load balancer ip you set.
* kubectl get deployments -o wide -n pihole

# Secret Filtering
* There is a go script that removes password/secret values from any yaml files

# Git Attributes
* This config tells git to run filter-secrets on any yaml files

# Git Config
* The filter secrets config here causes the files to be cleaned before being committed.
* Make sure the following is in your local .git/config if you plan to use the Go script.
```
[filter "secrets"]
    clean = ./filter-secrets
    smudge = cat
```