
## 1. Configure environment

### update the package index
```shell
$ sudo apt update
```

### Set hostname
```shell
$ hostnamectl set-hostname opsaio1
```

### Install Python build dependencies:
```shell
$ sudo apt install python3-venv python3-dev libffi-dev gcc libssl-dev vim
```

## Install dependencies using a virtual environmen

Create a virtual environment for deploying Kolla-ansible. To avoid conflict between system packages and Kolla-ansible packages.

### Create a virtual environment and activate it: 
```shell
$ python3 -m venv $HOME/openstack-aio
$ source $HOME/openstack-aio/bin/activate
```

### Ensure the latest version of pip is installed:
```shell
$ pip install -U pip
```

### Install Ansible. Kolla Ansible requires at least Ansible 2.9 and supports up to 2.10.
```shell
$ pip install 'ansible==2.9'
```

## Install Kolla-Ansible

### Install Kolla-ansible on virtual enviroment above

```shell
$ pip install kolla-ansible
```

## Configure Kolla-ansible for All-in-one OpenStack Deployment

### Next, create Kolla configuration directory;

```shell
$ sudo mkdir /etc/kolla
```

### Update the ownership of the Kolla config directory to the user with which you activated Koll-ansible deployment virtual environment as.

```shell
$ sudo chown $USER:$USER /etc/kolla
```

### Copy globals.yml and passwords.yml to /etc/kolla directory.
```shell

$ cp $HOME/openstack-aio/share/kolla-ansible/etc_examples/kolla/* /etc/kolla/
```

### Copy the main Kolla configuration file, ```globals.yml``` and the OpenStack services passwords file, ```passwords.yml``` into the Kolla configuration directory above from the virtual environment.
### Copy all-in-one and multinode inventory files to the current directory.

```shell
$ cp $HOME/openstack-aio/share/kolla-ansible/ansible/inventory/all-in-one .
```

### Configure ansible.cfg

```yaml
[defaults]
host_key_checking=False
pipelining=True
forks=100
```

## Define Kolla-Ansible Global Deployment Options

### Open the ```globals.yml``` configuration file and define the AIO Kolla global deployment options;

```shell
$ vim /etc/kolla/globals.yml
```

#### Below are the basic options that we enabled for our AIO OpenStack deployment.


- ```global.yml```

```yaml
kolla_base_distro: "ubuntu"
kolla_install_type: "source"
network_interface: ens33
neutron_external_interface: ens34
nova_compute_virt_type: "qemu"
enable_haproxy: "no"
enable_cinder: "yes"
enable_cinder_backup: "no"
enable_cinder_backend_lvm: "yes"
```

### Generate Kolla Passwords

**Kolla ```passwords.yml``` configuration file stores various OpenStack services passwords.**

**Automatically generate the password using the Kolla-ansible kolla-genpwd in your virtual environment:**

```shell
$ kolla-genpwd
```

## 2. Deploy All-In-One OpenStack

```shell
PLAY [Gather facts for all hosts] **********************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [Group hosts to determine when using --limit] *****************************
ok: [localhost]
[WARNING]: Could not match supplied host pattern, ignoring:
all_using_limit_True


PLAY [Gather facts for all hosts (if using --limit)] ***************************
skipping: no hosts matched

PLAY [Apply role baremetal] ****************************************************

TASK [baremetal : include_tasks] ***********************************************
included: /home/tienle/openstack-aio/share/kolla-ansible/ansible/roles/baremetal/tasks/bootstrap-servers.yml for localhost

TASK [baremetal : Ensure localhost in /etc/hosts] ******************************
changed: [localhost]

TASK [baremetal : Ensure hostname does not point to 127.0.1.1 in /etc/hosts] ***
changed: [localhost]

TASK [baremetal : Generate /etc/hosts for all of the nodes] ********************
changed: [localhost]

TASK [baremetal : Ensure /etc/cloud/cloud.cfg exists] **************************
ok: [localhost]

TASK [baremetal : Disable cloud-init manage_etc_hosts] *************************
skipping: [localhost]

TASK [baremetal : Ensure sudo group is present] ********************************
ok: [localhost]

TASK [baremetal : Ensure kolla group is present] *******************************
changed: [localhost]

TASK [baremetal : Install apt packages] ****************************************
[WARNING]: Could not find aptitude. Using apt-get instead

changed: [localhost]

TASK [baremetal : Install ca certs] ********************************************
ok: [localhost] => (item=ca-certificates)
changed: [localhost] => (item=apt-transport-https)

TASK [baremetal : Ensure apt sources list directory exists] ********************
ok: [localhost]

TASK [baremetal : Install docker apt gpg key] **********************************
changed: [localhost]

TASK [baremetal : Enable docker apt repository] ********************************
changed: [localhost]

TASK [baremetal : Ensure yum repos directory exists] ***************************
skipping: [localhost]

TASK [baremetal : Enable docker yum repository] ********************************
skipping: [localhost]

TASK [baremetal : Ensure module_hotfixes enabled for docker] *******************
skipping: [localhost]

TASK [baremetal : Install docker rpm gpg key] **********************************
skipping: [localhost]

TASK [baremetal : Update apt cache] ********************************************
changed: [localhost]

TASK [baremetal : Set firewall default policy] *********************************
ok: [localhost]

TASK [baremetal : Check if firewalld is installed] *****************************
skipping: [localhost]

TASK [baremetal : Disable firewalld] *******************************************
skipping: [localhost] => (item=firewalld) 

TASK [baremetal : Check which containers are running] **************************
ok: [localhost]

TASK [baremetal : Install apt packages] ****************************************

```

**.............**