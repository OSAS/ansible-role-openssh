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

# Filtering ciphers and various cryptographic algos

For security reasons, if people need to remove specific ciphers, this can be
achieved with the `filters` option. This can be used for now on cipers and MAC,
and requires to either give the exact name in the `to_remove` list, or a regexp
in `to_remove_re`.

For example, to remove arcfour and 2 md5 based mac (out of 3), the following
snippet can be used:
```
$ cat deploy_ssh.yml
- hosts: all
  roles:
  - role: openssh
    use_onion_service: True
    filters:
      ciphers:
        to_remove_re:
        - arcfour
      macs:
        to_remove:
        - hmac-md5-96
        - hmac-md5
```

Others options may be added, see vars/main.yml for the options that can be affected.
For now, only `ciphers` and `macs` are officialy supported. The others can have their name
changed.

The system try to be as safe as possible to avoid breakage since openssh is
critical for ansible, see the commit log for more precisions.

Since this requires a ssh client that support -Q argument for autodetection of
the supported ciphers, anything else will be untouched, out of conservatism.
