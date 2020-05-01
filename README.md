Ansible Role: Cobalt Strike
=========

An Ansible Role to install Cobalt Strike and to configure it either as Operator (client) or Teamserver (server).

If configured as Teamserver, it will start automatically with the supplied password and C2 profile.

If configured as Operator, it will download the artifact and resource kit automatically.

The role installs Cobalt Strike into a cobalstrike folder in your user's home directory `~/cobaltstrike`. Any additional downloads like kits and profiles are put in respective folders inside that directory, so `~/cobaltstrike/profiles` and `~/cobaltstrike/kits`.

Requirements
------------

None

Role Variables
--------------

| Variable                          | Default                 | Comments (type)                                                                                                                                                                       |
| :-------------------------------- | :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| cobalt_strike_role            | operator                   | Defaults to `operator`
| c2_profile_url            | https://github.com/xx0hcd/Malleable-C2-Profiles/raw/master/template.profile                   | Must be a URL
| teamserver_password            | password                   | password for your Teamserver
| license_key            | aaaa-bbbb-cccc-dddd                   | Must be a valid license key. Running with the default in this role will fail.

Dependencies
------------

None

Example Playbook
----------------

    - hosts: cobalt_strike_infrastructure
        tasks:
          - name: include cobalt_strike role
            include_role:
              name: cobalt_strike
            tags: always

Vars example for Teamserver

    c2_profile_url: https://github.com/xx0hcd/Malleable-C2-Profiles/raw/master/template.profile
    cobalt_strike_role: teamserver

You can execute the playbook with `--extra-vars` to avoid placing your license key or password in any code.

    ansible-playbook -i inventory cobaltstrike_playbook.yml --extra-vars '{"license_key":"aaaa-bbbb-cccc-dddd","teamserver_password":"password"}'

Installing as `operator` only requires a license key variable.

    ansible-playbook -i inventory cobaltstrike_playbook.yml --extra-vars '{"license_key":"aaaa-bbbb-cccc-dddd"}'



License
-------

MIT / BSD

Author Information
------------------

This role was created in 2020 by @chryzsh. 