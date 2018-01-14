
# Ansible X Docker X Packer
This repository is a template to demonstrate how to use Packer & Ansible to provision Docker image

# Prerequisite
There are several modules need to be installed before getting start
- HashiCorp Packer ([Installation Reference](https://www.packer.io/docs/install/index.html))
- Docker ([Installation Reference](https://docs.docker.com/engine/installation/))

# Get Start

First, Git clone this repository, and switch into folder

```
~$ git clone https://github.com/smalltown/ansibleXdockerXpacker
~$ cd ansibleXdockerXpacker
```

Use below Packer command to start provision nginx application docker image, and **replace #{image_tag}** with others you prefer

```
~$ packer build -var 'image_tag=#{image_tag}' packer.json
```

The provision process will show up in stdout like below

```
...

==> docker: Provisioning with Ansible...
    docker: Creating Ansible staging directory...
    docker: Creating directory: /tmp/packer-provisioner-ansible-local/5a5aeeb6-d8ee-c7a1-d91d-b893c8f96860
    docker: Uploading main Playbook file...
    docker: Uploading inventory file...
    docker: Uploading role directories...
    docker: Creating directory: /tmp/packer-provisioner-ansible-local/5a5aeeb6-d8ee-c7a1-d91d-b893c8f96860/roles/ansible
    docker: Executing Ansible: cd /tmp/packer-provisioner-ansible-local/5a5aeeb6-d8ee-c7a1-d91d-b893c8f96860 && ANSIBLE_FORCE_COLOR=1 PYTHONUNBUFFERED=1 ansible-playbook /tmp/packer-provisioner-ansible-local/5a5aeeb6-d8ee-c7a1-d91d-b893c8f96860/ansible.yml --extra-vars "packer_build_name=docker packer_builder_type=docker packer_http_addr="  -c local -i /tmp/packer-provisioner-ansible-local/5a5aeeb6-d8ee-c7a1-d91d-b893c8f96860/packer-provisioner-ansible-local687885156
    docker:
    docker: PLAY [provision ansibleXdockerXpacker docker image] ****************************
    docker:
    docker: TASK [Gathering Facts] *********************************************************
    docker: ok: [127.0.0.1]
    docker:
    docker: TASK [ansible : update apt cache.] *********************************************
    
...

==> Builds finished. The artifacts of successful builds are:
--> docker: Imported Docker image: sha256:f7470a729d0e237fe03fc0dcf2c4d37f1a140aa9b504fbed64b6a4ff6d5bd954
--> docker: Imported Docker image: smalltown/ansiblexdockerxpacker:#{image_tag}

```

Once packer execute completely, there is a nginx application docker images store  local

```
~$ docker images

REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
smalltown/ansiblexdockerxpacker   #{image_tag}        f7470a729d0e        10 minutes ago      358MB
ubuntu                            xenial              00fd29ccc6f1        4 weeks ago         111MB
```

The images also can be pushed to the remote docker registry, using [docker-push](https://www.packer.io/docs/post-processors/docker-push.html) of Packer to achieve it