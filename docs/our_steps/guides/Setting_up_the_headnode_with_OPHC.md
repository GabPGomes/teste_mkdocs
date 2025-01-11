# Setting up the headnode with OPHC

```bash
yum install git vim bash-completion

# this is the repo with the playbooks
git clone --single-branch --branch 2.0-release https://github.com/XSEDE/CRI_XCBC/

ssh-keygen  # bla bla bla
cat .ssh/id_rsa.pub >> .ssh/authorized_keys

cd ./CRI_XCBC
./install_ansible.sh
```