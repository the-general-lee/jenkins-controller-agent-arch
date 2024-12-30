# jenkins-controller-agent-arch
## Description
This project is a demonstration of a Jenkins Controller-Agent Architecture configured with **two agent nodes**:
- Node 1: Dedicated to building a **Node.js** [application](https://github.com/the-general-lee/node_docker).
- Node 2: Dedicated to building an **Angular** [application](https://github.com/the-general-lee/angular_docker).

The setup illustrates the division of tasks in a Jenkins environment, showcasing how different builds can be executed on separate agents for better scalability and resource management.

This example is ideal for understanding distributed build architectures and integrating Jenkins into CI/CD pipelines for modern web applications.

## Architecture
Jenkins Controller-Agent Architecture can be set up where the agent nodes can be:
- Bare Metal Machines
- Virtual Machines
- Container (static/dynamic)

The static container approach has been replaced in recent years by more and more plugins allowing dynamic containers (destroyed upon pipelines completions).

In this project, however, static containers are highlighted to dive deeper into the architecture and extract richer learning outcomes.

Master node communicate jobs to these static container agents through ssh, the architecture is as follows:
- Jenkins controller (ssh client): retains the private key to authenticate with all available nodes.
- Jenkins agents (ssh servers): contain the public key to compare with each attemp from the controller.

## Prerequisites

## Usage
