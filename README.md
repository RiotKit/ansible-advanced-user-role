Multi-User project
==================

Multi-user management with focus on a project to which everybody has access via sudo.

Example case:
- There are multiple organizations managing their pages
- We deploy a docker project that contains multiple websites
- Each user can manage the docker project via `sudo ./make.sh ... something ...` instead of having access to global sudo


Role Variables
--------------

```yamlex
technical_entrypoint: "/project/make.sh"
enable_technical_entrypoint: true

technical_account: "tech.admin"
technical_account_id: 1800
technical_group: "technical"
technical_group_id: 1161

users:
    accounts:
        - login: iwa.somebody
          section: "ZSP" # account description / organization name / etc.
          password: 'some-password-hash-generated-by-mkpasswd'
          global_sudo: no
          gid: 1161
          uid: 2050
          disabled: no
```

Example Playbook
----------------

```yamlex
    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }
      vars:
         # ...
```

Adding a new user account
-------------------------

1. Use the tool `./mkpasswd.sh` to generate a password
2. Create an entry in the users.accounts variable (there are examples already)
    - Paste the password into password section of your new account with single quotes
    - Please do not enable `global_sudo` option unless you really have a reason for that
    - Please fill in `section` field with the organization name
    - Please use only a-z, numbers and dot characters for the user name, else it may not work
3. Run deployment

Blocking access for the user account
------------------------------------

1. Edit users.accounts variable
2. For specified user account please set `disabled: yes` 
    - NOTICE: Deleting whole user section from file will not have an effect, as the deployment will ignore that user and will not change it
      so the user account deletion is not possible, only blocking is possible
3. Run deployment

License
-------

MIT

Author Information
------------------

Krzysztof Wesołowski, anarchosyndicalist, backend-devops programmer, grassroot advocate

Made especially for:
https://iwa-ait.org
https://zsp.net.pl
