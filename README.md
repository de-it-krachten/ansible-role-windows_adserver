[![CI](https://github.com/de-it-krachten/ansible-role-windows_adserver/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-windows_adserver/actions?query=workflow%3ACI)


# ansible-role-windows_storage

Installs Windows AD server.
Purpose ofg this role is to build an AD server to be used for Linux client testing.



## Dependencies

#### Roles
None

#### Collections
- community.general
- ansible.windows
- microsoft.ad
- ansible.windows
- microsoft.ad

## Platforms

Supported platforms

- Windows 2012 R2<sup>1</sup>
- Windows 2016<sup>1</sup>
- Windows 2019<sup>1</sup>
- Windows 2022<sup>1</sup>

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Windows features to install
windows_adserver_features:
  # - Web-Server
  - AD-domain-services

# AD domain settings
windows_adserver_domain_settings:
  dns_domain_name: "{{ windows_adserver_domain }}"
  safe_mode_password: "{{ windows_adserver_safe_mode_password }}"
  create_dns_delegation: no
  database_path: C:\Windows\NTDS
  domain_mode: WinThreshold
  domain_netbios_name: ANSIBLE
  forest_mode: WinThreshold
  sysvol_path: C:\Windows\SYSVOL

# List of AD users that should exist
windows_adserver_users: []

# List of AD groups that should exist
windows_adserver_groups: []
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'windows_adserver'
  hosts: all
  become: 'yes'
  vars:
    windows_adserver_domain: example.com
    windows_adserver_safe_mode_password: Passw0rd!
    windows_adserver_groups:
      - name: group1
        attributes:
          set:
            gidNumber: 5000
      - name: group2
        attributes:
          set:
            gidNumber: 5001
      - name: group3
        attributes:
          set:
            gidNumber: 5002
      - name: group4
        attributes:
          set:
            gidNumber: 5003
    windows_adserver_users:
      - name: user1
        password: password123!
        groups:
          add:
            - group1
        attributes:
          set:
            uidNumber: 5000
            gidNumber: 5000
      - name: user2
        password: password123!
        groups:
          add:
            - group2
        attributes:
          set:
            uidNumber: 5001
            gidNumber: 5001
      - name: user3
        password: password123!
        groups:
          add:
            - group1
            - group2
        attributes:
          set:
            uidNumber: 5002
            gidNumber: 5002
      - name: user4
        password: password123!
        attributes:
          set:
            uidNumber: 5003
            gidNumber: 5003
  tasks:
    - name: Include role 'windows_adserver'
      ansible.builtin.include_role:
        name: windows_adserver
</pre></code>
