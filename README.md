Ansible role common-packages
=====

variables of the role
-----

### Variable autoremove_packages

- `autoremove_packages` - yes/no. Only for package manager: apt|dnf, meets the requirements of **autoremove**.

### Variables list_packages / list_services

- `host_group_names_inventory` - the parameter lists the list of servers or groups from inventory on which the package should be installed. ** The absence of a parameter ** is equivalent to specifying `all`, the package is installed on all servers.
- `os_family` - the parameter corresponds to the facts from` ansible_os_family`. Lists are supported. ** Missing a parameter ** is equivalent to specifying `all`.
- `os_distribution` - the parameter corresponds to the facts from` ansible_distribution`. Lists are supported. ** Missing a parameter ** is equivalent to specifying `all`.
- `os_distribution_major_version` - the parameter corresponds to the facts from` ansible_distribution_major_version`. Lists are supported. ** Missing a parameter ** is equivalent to specifying `all`.

### Variable list_packages

Supplementary to your specified ** optional ** fact and inventory attributes for `list_packages` are supported:

- `packages_manager` - the parameter corresponds to the facts from` ansible_pkg_mgr`. Lists are supported. ** Missing a parameter ** is equivalent to specifying `all`.


Supported section attributes `packages`:

- `name` - name of package.
- `state` - optionaly, the default value is `present`, supports values  `present`/`latest`/`absent`

Variable example:

```yaml
list_packages:
  - name: "Packages for All servers"
    packages:
      - name: package_name1
        #state: present (по умолчанию, если не указано)
      - name: package_name2
        state: latest
      - name: package_name3
        state: absent
  - name: "Packages for All RedHat servers (<= 7)"
    packages_manager: yum
    packages:
      - name: package_name4
  - name: "Packages for All RedHat servers (>= 8)"
    packages_manager: dnf
    packages:
      - name: package_name4
  - name: "Packages for All Deb servers"
    packages_manager: apt
    packages:
      - name: package_name4
  - name: "Packages for Specific servers"
    os_distribution: Ubuntu
    packages:
      - name: package_name5
  - name: "Packages for Specific servers"
    packages_manager: apt
    os_distribution_major_version:
      - "16"
      - "18"
    packages:
      - name: package_name6
  - name: "Packages for Specific servers"
    packages_manager: apt
    os_distribution_major_version: "18"
    host_group_names_inventory:
      - group1
      - group2
    packages:
      - name: package_name7
```

### variable list_services

Supported attributes of the services section, the values ​​of which correspond to the parameters of the service module in ansible:

- `name`
- `enabled`
- `state`
- `runlevel`
- `sleep`
- `arguments`
- `use`
- `pattern`

Variable example:

```yaml
list_services:
  - name: "Services on All servers"
    services:
      - name: service_name1
        enabled: yes
        state: started
      - name: service_name2
        enabled: yes
        state: started
      - name: service_name3
        enabled: yes
        state: started
  - name: "Services on All RedHat servers (<= 7)"
    services_manager: yum
    services:
      - name: service_name4
        enabled: yes
        state: started
  - name: "Services on All RedHat servers (>= 8)"
    services_manager: dnf
    services:
      - name: service_name4
        enabled: yes
        state: started
  - name: "Services on All Deb servers"
    services_manager: apt
    services:
      - name: service_name4
        enabled: yes
        state: started
  - name: "Services on Specific servers"
    os_distribution: Ubuntu
    services:
      - name: service_name5
        enabled: yes
        state: started
  - name: "Services on Specific servers"
    services_manager: apt
    os_distribution_major_version:
      - "18"
    services:
      - name: service_name6
        enabled: yes
        state: started
  - name: "Services on Specific servers"
    services_manager: apt
    os_distribution_major_version: "18"
    host_group_names_inventory:
      - group1
      - group2
    services:
      - name: service_name7
        enabled: yes
        state: started
```

Example playbook
-----

```yaml
- name: Install packages
  become: true
  roles:
    - common-packages
```

