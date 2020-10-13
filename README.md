Ansible Role: Cobalt Strike
=========

An Ansible Role to install Cobalt Strike and to configure it either as Operator (client) or Teamserver (server).

If configured as Teamserver, it will start automatically with the supplied password and C2 profile.

If configured as Operator, it will download the artifact and resource kit automatically.

The role installs Cobalt Strike into a cobalstrike folder in your user's home directory `~/cobaltstrike`. Any additional downloads like kits and profiles are put in respective folders inside that directory, so `~/cobaltstrike/profiles` and `~/cobaltstrike/kits`.

The script includes options to use [C2Concealer](https://github.com/FortyNorthSecurity/C2concealer) to request a Let's Encrypt for your C2 domain, and supply hostnames for use with custom domains, for example with domain fronting.

Requirements
------------

None

Role Variables
--------------

| Variable                          | Default                 | Comments (type)                                                                                                                                                                       |
| :-------------------------------- | :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| cobalt_strike_role            | operator                   | Defaults to `operator`
| teamserver_password            | password                   | password for your Teamserver
| license_key            | aaaa-bbbb-cccc-dddd                   | Must be a valid license key. Running with the default in this role will fail.
| cdn_endpoint            | example.com                   | If using a CDN endpoint for the Host Header in the C2 profile, provide the URL to it
| variants            | 1                   | CS C2 profile variants in C2Concealer
| c2concealer_option            | 2                   | Selects Let's Encrypt option in C2Concealer
| c2_hostname            | example.com                   | Hostname to generate TLS certificate for. Should be your real C2 server.
| keystore_password            | Welcome1                  | Keystore password for TLS cert generated in C2Concealer
| c2_profile_filename            | template.profile                  | Specify the file name of a custom C2 profile to start the Teamserver with. Must be placed in the profiles directory in the cobaltstrike directory. The template.profile file does not exist by default.

Dependencies
------------

[C2Concealer](https://github.com/FortyNorthSecurity/C2concealer), but will be installed automatically on the Teamserver.

Example Playbook
----------------

    - hosts: cobalt_strike_infrastructure
        tasks:
          - name: include cobalt_strike role
            include_role:
              name: cobalt_strike
            tags: always

Example where only certain tasks from the role is run. Here: tasks/configure-teamserver.yml

    - hosts: cobalt_strike_infrastructure
        tasks:
          - name: include cobalt_strike role
            import_role:
              name: cobalt_strike
              tasks_from: configure-teamserver
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