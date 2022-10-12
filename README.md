# Testing-Academy enviroment

The purpose of this repository is to create a fully functional virtual enviroment:
 - running on Kubernetes Cluster,
 - with private docker registry,
 - Jenkins with self-update job,
 - ingress with MetalLB(solution for local use of K8s),
 - bash script to automate the start-up process.


## You will need:

#### Hardware:

x | Recommended | Minimum
----------- | ----------- | ----------- 
Available RAM | 16 GB | 8 GB
CPUs | 8 cores | 4 cores

#### Software:

 - [Vagrant](https://www.vagrantup.com/downloads)
 - [Virtualbox](https://www.virtualbox.org/wiki/Downloads)

## Download

Clone this repository to your local machine:

`git clone https://github.com/boledovicpantheon/testing-academy.git` 
Or just simply download and extract it.


## Configuration

##### 1. Vagrantfile

Here you can edit the hardware requirements.   

`*_NODES` = number of nodes to create,
`*_RAM` = amount of RAM in MB for each of the node,
`*_CPUS` = number of CPU cores to attach to each node.


The worker node server as a slave - it will be executing almost all tasks.
The master node, serves as a control node/plane, thats why it needs more RAM and CPUs. 

## Running the enviroment



2. From inside the repository, run 
`vagrant up`

3. Add DNS entry to your local machine with IP `192.168.56.240` and DNS name`example.jenkins.com`.
This way you will be able to access Jenkins controller on your local machine. It runs on port 80, so so need to specify it.

#### Optional:

4. Private docker registry is accesible only inside cluster, if you want to access it from your local machine also, you need to add IP 
`192.168.56.11` and DNS name `docker.registry.com` to your `/etc/hosts`, or alternative.
Don't forget that docker registry runs on port 5000. 


### Start-up should take no longer than 30 minutes, but it depends on your hardware capacity.

After successfull startup, you will have: 


TODO
