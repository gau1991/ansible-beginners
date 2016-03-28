## Ansible Demo, Do it your self

### Requirements:

1. Ubuntu Based system

### Steps:

#### 1. Setup Vagrant Machine (Optional):

1. Install Vagrant and VirtualBox

  ```
  ## VirtualBox
  sudo apt-get update
  sudo apt-get install virtualbox
  wget https://releases.hashicorp.com/vagrant/1.8.1/vagrant_1.8.1_x86_64.deb
  sudo dpkg -i vagrant_1.8.1_x86_64.deb
  ```
2. Add Ubuntu Vagrant box and init Vagrant File

  ```
  mkdir work && cd work
  vagrant init ubuntu/trusty64; vagrant up --provider virtualbox
  ```

3. SSH into Vagrant

  ```
  vagrant ssh
  ```

#### 2. SSH Keys and passwordless access

1. Generate your ssh keys
  ```
  ssh-keygen
  ```
2. Copy public key for passwordless access

  ```
  cd ~/.ssh
  cat id_rsa.pub >> authorized_keys
  ```
3. Install OpenSSH packages
  ```
  sudo apt-get update
  sudo apt-get install openssh-server
  ```

#### 3. Install Ansible

```
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update
sudo apt-get install -y ansible
```

#### 4. Setup Inventory

1. First backup your inventory file
  ```
  sudo mv /etc/ansible/hosts /etc/ansible/hosts.orig
  ```

2. Create a new inverntory file
  ```
  sudo vim /etc/ansible/hosts
  ```
3. And add following content init
  ```
  [local]
  127.0.0.1
  ```

#### 5. Try Various Modules:

```
$ansible all -m ping
$ansible all -m setup
$ansible all -m command -a "ls"
$ansible all -m command -a "df -h"
$ansible all -m shell -a "ls"
$ansible all -m shell -a "ls | grep txt"
$ansible all -b --ask-become-pass -m apt -a "name=htop update_cache=yes"
```

#### 6. Try running Nginx Playbook:
1. Create web.yml file

  ```
  vim web.yml
  ```

2. Add following content in it

  ```
      ---
    - hosts: local
      vars:
       - docroot: /var/www/serversforhackers.com/public
      tasks:
       - name: Add Nginx Repository
         apt_repository: repo='ppa:nginx/stable' state=present
         register: ppastable

       - name: Install Nginx
         apt: pkg=nginx state=installed update_cache=true
         when: ppastable|success
         register: nginxinstalled
         notify:
          - Start Nginx

       - name: Create Web Root
         when: nginxinstalled|success
         file: dest={{ '{{' }} docroot {{ '}}' }} mode=775 state=directory owner=www-data group=www-data
         notify:
          - Reload Nginx

      handlers:
       - name: Start Nginx
         service: name=nginx state=started

        - name: Reload Nginx
          service: name=nginx state=reloaded
  ```

3. Run the playbook

  ```
  ansible-playbook -b --ask-become-pass web.yml
  ```
