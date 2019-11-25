Multi-User project
==================

Multi-user management with focus on a project to which everybody has access via sudo.

**Features:**
- User creation
- Jailing using docker (runs a one-time docker container for a ssh session) (optional)
- Giving limited sudo access to one command for project management (optional)
- ZSH configuration with oh-my-zsh extensions (optional)
- Optional SSH configuration per user (eg. what is allowed, if user can forward ports, if can forward X11, etc.) (optional)

**Example case #1:**
- There are multiple organizations managing their pages
- We deploy a docker project that contains multiple websites
- Each user can manage the docker project via `sudo ./make.sh ... something ...` instead of having access to global sudo


Role Variables
--------------

```yamlex
technical_entrypoint: "/project/make.sh"
enable_technical_entrypoint: true

use_technical_group: true
use_technical_user: true
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
          ssh_pub_key: 'path-to-id.pub'
          ssh_priv_key: 'path-to-id'
          ssh_authorized_keys:
              - path_to_key.pub
          ssh_known_hosts:
              - "[localhost]:2222 ecdsa-sha2-nistp256 soooomeeekey-here"
          gid: 1161
          uid: 2050
          disabled: no
          shell: /bin/zsh

          # optional jail configuration (defaults: no jail usage)
          jailed: no
          containerize_image: "alpine:3.10"

          # optional SSH configuration per user (defaults: global ssh settings used, nothing overridden if key here is not defined)
          tcp_forwarding: yes
          x11_forwarding: yes
          allow_password_auth: yes
          gateway_ports: yes
          permit_tty: yes
          permit_tunnel: yes
          allow_agent_forwarding: yes
          permit_user_environment: yes
          client_alive_interval: 30
          client_alive_count_max: 2
          disable_sftp: yes
          #ssh_force_command: /bin/false # does not work when "jailed: yes"
```

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
