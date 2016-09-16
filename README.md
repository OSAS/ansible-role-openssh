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

# Deploying a onion service

If one wish to deploy a onion service to access the ssh server, the role
can automate that part with the option `use_onion_service`, like this:
```
$ cat deploy_ssh.yml
- hosts: all
  roles:
  - role: openssh
    use_onion_service: True
```

The generated hostname will be in `/var/lib/tor/onion_service_ssh/hostname`
