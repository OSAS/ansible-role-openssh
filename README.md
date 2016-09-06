Ansible module used by OSAS to manage sshd.

It only enforce basic security policy for now, using lineinfile.

# Lineinfile usage

Since openssh is critical to ansible and sysadmin operations, and
since we manage a wide range of servers with
varying features and OS, there is no current attempt to use a template
for the configuration file.

# Usage

```
$ cat deploy_ssh.yml
- hosts: all
  roles:
  - role: openssh
```
