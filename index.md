---

layout: ribbon

style: |

    #Cover h2 {
        margin:30px 0 0;
        color:#FFF;
        text-align:center;
        font-size:70px;
        }
    #Cover p {
        margin:10px 0 0;
        text-align:center;
        color:#FFF;
        font-style:italic;
        font-size:20px;
        }
        #Cover p a {
            color:#FFF;
            }
    #Picture h2 {
        color:#FFF;
        }
    #SeeMore h2 {
        font-size:100px
        }
    #SeeMore img {
        width:0.72em;
        height:0.72em;
        }
    #Handlers code {
        margin:0 0 0;
        line-height:40px;
        }

---

# Ansible for Beginners {#Cover}

*Brought you by [Gaurav Ashtikar](http://gau1991.me/)*

![](pictures/cover.jpg)
<!-- photo by WikiPedia, https://en.wikipedia.org/wiki/Server_%28computing%29#/media/File:Wikimedia_Foundation_Servers-8055_35.jpg CC BY-SA 3.0 -->


## Contents

1. Why Ansible?
2. Installation and Basic setup
3. Basic Modules
4. Basic of Playbooks

## Why Ansible

1. Simple to use
2. Leverages Python
3. Modular
4. Open Source
5. Support multi tier deployment

{:.note}
Ansible: fictional machine capable of instantaneous or superluminal communication.
<!-- Courtesy: Wikipedia -->

## Installation

1. Using APT (On Ubuntu)
    - `$sudo apt-add-repository ppa:ansible/ansible`
    - `$sudo apt-get update`
    - `$sudo apt-get install ansible`

2. Using YUM/DNF (On RHEL/CentOS/Fedora)
    - `$sudo yum install ansible`

3. Using PIP (On Mac OS/Other Linux distros)
    - `$sudo easy_install pip`
    - `$sudo pip install ansible`

## Inventory file

    mail.example.com

    [webservers]
    foo.example.com
    bar.example.com

- Default file is `/etc/ansible/hosts`

## SSH-Keys

1. Generate SSH key using `$ssh-keygen`
2. Make sure you have access to remote system
3. Add your public key (`~/.ssh/id_rsa.pub`) into remote `~/.ssh/authorized_keys` file


## Test your setup

1. `$ansible all -m ping`
3. `$ansible all -m ping -u root -k`
4. `$ansible all -m ping -K`
5. `$ansible all -m ping -f 10`

## Modules

1. Libraries that can be executed directly on remote hosts or through Playbooks.
2. User can also write their own modules
3. Two types of modules:
  - [Core Modules](https://github.com/ansible/ansible-modules-core)
  - [Extras Modules](https://github.com/ansible/ansible-modules-extras)

## Command Modules
  - `$ansible all -m command -a "ls"`
  - `$ansible all -m command -a "df -h"`
  - `$ansible all -m shell -a "ls"`
  - `$ansible all -m shell -a "ls | grep txt"`
  - `$ansible all -m shell -a "apt-get update"`

## System Modules:
  - `$ansible all -m ping`
  - `$ansible all -m setup`
  - `$ansible all -m hostname -a "name=web01"`
  - `$ansible all -m service -a "name=nginx state=started"`
  - `$ansible all -m user -a "name=jsmith generate_ssh_key=yes ssh_key_bits=2048 ssh_key_file=.ssh/id_rsa"`


## File Modules:
  - `$ansible all -m copy -a "src=/srv/myfiles/foo.conf dest=/etc/foo.conf"`
  - `$ansible all -m find -a "paths=/tmp patterns=*.old"`
  - `$ansible all -m stat -a "path=/path/to/something checksum_algorithm=sha256"`


## Source Control Modules:
  - `$ansible all -m git -a "repo=git://git@github.com/mylogin/hello.git dest=/home/mylogin/hello"`
  - `$ansible all -m git -a "repo=git://foosball.example.org/path/to/repo.git dest=/srv/checkout clone=no update=no"`


## Packaging Modules:
  - `$ansible all -m apt -a "name=htop update_cache=yes"`
  - `$ansible all -m apt -a "name=htop state=latest"`
  - `$ansible all -m apt -a "name=htop state=absent"`
  - `$ansible all -m apt -a "upgrade=dist"`

## Playbooks

Basic Playbook `nginx.yml` example:

    ---
    - hosts: local
    tasks:
      - name: Install Nginx
        apt: pkg=nginx state=installed update_cache=true

To run Playbook : `$ansible-playbook -b nginx.yml`

## Handlers {#Handlers}
{: .slide .w }

    ---
    - hosts: local
    tasks:
      - name: Install Nginx
        apt: pkg=nginx state=installed update_cache=true
        notify:
          - Start Nginx
    handlers:
      - name: Start Nginx
        service: name=nginx state=started

## Demo Time

  [Do it yourself](https://github.com/gau1991/ansible-beginners/blob/gh-pages/demo.md)

## References

1. http://docs.ansible.com/ansible
2. https://serversforhackers.com/an-ansible-tutorial
3. http://wiki.glitchdata.com/index.php?title=Ansible_Training

## **Thanks**
