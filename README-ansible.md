# Ansible Setup in Windows 10 - ubuntu app

sudo apt update

sudo apt install software-properties-common

sudo apt-add-repository --yes --update ppa:ansible/ansible

sudo apt install ansible

$ ansible --version
ansible 2.9.7
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/ksomalin/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.17 (default, Apr 15 2020, 17:20:14) [GCC 7.5.0]


If you don’t have pip installed in your version of Python, install it:

sudo apt install python-pip

To install from source, clone the Ansible git repository:
git clone https://github.com/ansible/ansible.git

cd ansible

cp requirement.txt ../.

pip install --user -r ./requirements.txt

echo "127.0.0.1" > ~/ansible_hosts

export ANSIBLE_INVENTORY=~/ansible_hosts

ansible all -m ping --ask-pass