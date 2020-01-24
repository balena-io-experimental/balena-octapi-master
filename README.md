# balena-octapi-master

### Introduction
This is a containerized version of the [OctaPi project](https://projects.raspberrypi.org/en/projects/build-an-octapi/1), which creates a distributed Raspberry Pi cluster to run computations that take advantage of the CPU and RAM on many physical devices. It uses Python and _dispy_ at its core. With this implementation, you can turn any balenaCloud device into a worker node without having to manually flash SD cards for a single-use IoT implementation.

OctaPi uses the term "Client" to define the machine on which you run your Python scripts. It sends jobs to what OctaPi calls "Servers," which return their results to the Client node. For better clarity in the balenaCloud use case, I've renamed the "Client" to "Master", and "Server" to "Worker". Please use [this repository](https://github.com/jtonello/balena-octapi-worker) to deploy Worker nodes.

### Installation
Deploying the master is straightforward, with a _docker-compose.yml_ and associated _Dockerfile.template_. The latter primarily provides the ability to run sshd, and makes the OctaPi scripts available.

The master works by SSH-ing into each worker node, so you need to seed the master and workers with SSH keys. You can reuse these examples or generate your own by running _ssh-keygen_ (without a password) and adding the id_rsa.pub key to _authorized_keys_ on the workers. The .ssh/config file disables StrictHostKeyChecking so initial logins by the master to the workers are non-interactive.
```
/balena-octapi-master/
├── docker-compose.yml
├── Dockerfile.template
├── start_dispynode.yml
├── README.md
├── .ssh
│   ├── authorized_keys
│   ├── config
│   ├── id_rsa
│   └── id_rsa.pub
```
### Deploy
Clone this repository, change into the balena-octapi-master directory and push to your application:
```
 $ git clone git@github.com:balena-io-playground/balena-octapi-master.git
 $ cd balena-octapi-master
 $ balena push <appname>
```
### Running computations
The default OctaPi Python scripts (computer.py, etc.) are placed in the /usr/src/app working directory and can be run with commands like the following:
```
$ python3 ./compute.py
```
Access the octapi-master service shell using _balena ssh <app name>_ or directly from the balenaCloud dashboard.

For details on the various Python scripts, please references the original OctaPi Guide at [Build an OctaPi](https://projects.raspberrypi.org/en/projects/build-an-octapi/1).

 
