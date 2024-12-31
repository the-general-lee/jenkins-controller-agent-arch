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

*The static container approach has been replaced in recent years by more and more plugins allowing dynamic containers (destroyed upon pipelines completions).*

In this project, however, `static containers` are highlighted to dive deeper into the architecture and extract richer learning outcomes.

Master node communicate jobs to these static container agents through ssh, the architecture is as follows:
- **Jenkins controller (SSH-Client)**: retains the private key to authenticate with all available nodes.
- **Jenkins agents (SSH-Servers)**: contain the public key to compare with each attemp from the controller.

## Prerequisites
- Add Credentials for github to pull the repo by controller node.
- Add Credentials for dockerhub to push application images by agent nodes.
- Create a .env file to hold the public key `JENKINS_AGENT_SSH_PUBKEY` referenced by agent images (SSH public key).
- Add Private key in jenkins as Credentials to be used by controller node.
- Create the private and public keys, avoiding using rsa since jenkins automation doesn't accept it, it gives all kinds of errors
`ssh-keygen -t ed25519 -f rsa_agent`

- For debugging you can `exec` into the master container and `ssh -i home/.ssh/key jenkins@dockerIp` into any agent

- For symmetry I used a controller container (I didn't rely on the virtual machine to act as the jenkins controller), following permissions need to be granted:
  - In the ***controller container*** `git config --global --add safe.directory '*'`
  - In each ***agent container*** you need to run `groupadd -g 997 docker && usermod -aG docker jenkins`

- `apt-get install -y chromium` needed to allow karma unit-testing inside the angular applications

*where 997 was the docker group number in my original VM so this is actually really catered to my project you should check your docker 
group number on your original machine and paste it in the previous command*

## Usage
1. `docker-compose up master` to turn on the controller node.
2. `docker-compose up nodejs` and `docker-compose up angular` to turn on the two agent nodes.
3. proceed to jenkins instance run by controller container and run each pipeline.
  1. Each pipeline references one of the repos linked for the node.js and angular apps.
  2. Each repo contains a Jenkinsfile with build, test and deployment stages.
  3. Each Jenkinsfile contains the name of the agent node that is going to run it.
4. Each agent node is prepared with the required environments and libraries needed to build, test and deploy its type of apps.
