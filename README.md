Users
=========

An ansible role for the management of users

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------
### User Directory
The main variable is `users_directory`, which is a dictionary of users and settings that is passed to `ansible.builtin.user`

See [ansible documentation](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html) for more information on what each setting does

```yaml
  username1:
    append: "{{ users_default_append }}"
    comment:
    createhome: "{{ users_default_createhome }}"
    expires:
    force: "{{ users_default_force }}"
    group:
    groups:
      - group1
      - group2
    home:
    move_home: "{{ users_default_move_home }}"
    password:
    remove: "{{ users_default_remove }}"
    shell: "{{ users_default_shell }}"
    skeleton: "{{ users_default_skeleton }}"
    state:
    uid:
    authorized_keys:
      - pubkey1
      - pubkey2
  username2:
    ...
  username3:
    ...
```
authorized_keys are passed to `ansible.posix.authorized_keys`

### Defaults for User Settings
The following `users_default_` variables can be set to effectively override the `ansible.builtin.user` defaults

|Variable|Default|Description|
|-----|-----|-----|
|users_default_append|false|if false, remove all non-specified groups|
|users_default_createhome|true||
|users_default_move_home|false|attempt to move the users old home to the specified directory|
|users_default_remove|false|if state is absent, attempt to remove associated directories|
|users_default_force|false|if sate is absnet, force removal of user and associated directories|
|users_default_shell|'/bin/bash'||
|users_default_skeleton|*unset*|path to skeleton directory|

### Sudo group
For convenience, the variable `users_sudo_groupname` is provided, and is set to either 'sudo' or 'wheel' depending on OS-family.  This variable can be used in `users_dictionary` definitions.  For example

```yaml
  users_dictionary:
    ben:
      groups:
        - "{{ users_sudo_groupname }}"
```

Dependencies
------------

none

Example Playbook
----------------
```yaml
    - name: Configure users
      hosts: all
      become: true
      vars:
        users_directory:
          ben:
            comment: 'ben@dataraven.co.uk (created by Ansible)'
            groups:
              - sudo
            authorized_keys:
              - 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC9V3zXcz+IrAlNeTSv33NTRoQjSWuCxiOIjmydnckBRG/3Tu8Hu6kRzcCuaxQ7MsAi+XZEbp8nlHw4I+5CGc45ijG1ofNLW4cgvdAeG9+UGDoB1DDSGZriUU/HSuIQA2NdxwgpuVV7uJJs4izQLHZNYRKoca8mtiGD14C0rZpA4z08I9mX7g3cG42sRuMWeiJ67CEoTROgOUh0wUfmJKBAdXjhT+CNpHst9AQwDoz2AeNggv0999n5/Xr8TJs7DVRH9m40oFgsZFOrPpBhm9OKCpmdJnSsqox8U7EvR2TpxdVDi3+k+tc3BN21ytUr25I58PN+djP5eanVcb/K8+TCPH204tJ/qoK/Yyep6p0eOr3b2t6i3rA1I9L7W+4R/RrqMfuqyA42qQKIxcHq0DGCwbmg4ab4ZQN3bNwoDFXT/ODiZ6cbVVe3BqOyudHHH/VXd6eibrQIbn+lZK6g23qgu0gHt7fB7qg5BrlQvKtdsG6JP60oNET4cdEbB+0fbwJMOdXNsPoZDUtqHBxKxdR7yR1SYKaNWN94QHMxcS8pX8YEuqgb2x5u3/CX7A+drSuxLLWOfJLY8v1cWlnZsxt4GXhZBOdMPNZ3sQ4pODpV8tOIoxvciv+gPunQAfQX9NzOVhhLRjaKTGOkk53EI8lRoFAaAhZfguZjlakJuVOewQ== ben@dataraven.co.uk'
      roles:
         - dataraven.users
```
License
-------

BSD-3-Clause license

Author Information
------------------

Ben Albon - ben@dataraven.co.uk
