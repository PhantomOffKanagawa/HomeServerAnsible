# Installation
- Ansible cannot use Windows as the control node (Took longer to figure that out than should have)
- Installed on wsl Ubuntu [Installing Ansible — Ansible Community Documentation](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu)
```bash
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```
# QuickStart
> Working in `/personal/ansible/` on WSL instance
- [Start automating with Ansible — Ansible Community Documentation](https://docs.ansible.com/ansible/latest/getting_started/get_started_ansible.html#get-started-ansible)
## Inventory
> Managed nodes in centralized file, manage multiple hosts with one command
> Admittedly not my focus with just one server atm
- Takes IP address or fully qualified domain name `host.example.com`
- In `inventory.yaml` or `.ini` file
### Groups
- Create groups of like hosts
```yaml
my_server:
  hosts:
    phantom_server:
      ansible_host: phantomserver
```
- Ensure correct format and addresses with cli
	- Pass `-u` option to `ansible` if username is different on control node than managed node(s) 
```bash
ansible-inventory -i inventory.ini --list
ansible myhosts -m ping -i inventory.ini
```
### Metagroups
> Group hosts by What, Where, and When and use uniform case sensitive names
```yaml
leafs:
  hosts:
    leaf01:
      ansible_host: 192.0.2.100
    leaf02:
      ansible_host: 192.0.2.110

spines:
  hosts:
    spine01:
      ansible_host: 192.0.2.120
    spine02:
      ansible_host: 192.0.2.130

network:
  children:
    leafs:
    spines:

webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
    webserver02:
      ansible_host: 192.0.2.150

datacenter:
  children:
    network:
    webservers:
```
### Variables
```yaml
webservers:
  hosts:
    webserver01:
      ansible_host: 192.0.2.140
      http_port: 80
    webserver02:
      ansible_host: 192.0.2.150
      http_port: 443
  vars:
    ansible_user: my_server_user
```
## Playbook
> Automation blueprints in `yaml` format
> Play: Ordered list of tasks that maps to nodes
> Task: Reference to single module that defines operations that Ansible performs
> Module: Unit of code or binary that Ansible runs on managed modules
```yaml
- name: baby's first play
  hosts: my_server
  tasks:
  - name: Ping hosts
    ansible.builtin.ping:

  - name: Hello world
    ansible.builtin.debug:
      msg: Hello world
```
