# Ansible role confluence

Installs [Atlassian Confluence](https://www.atlassian.com/software/confluence/) on RedHat/CentOS or Ubuntu

Default Confluence will be configured to be used via a reverse proxy server. Default Jira will be configured to be used via a reverse proxy server. So after the first / fresh provision navigate to Confluence via the reverse proxy and complete the setup.

<!-- MarkdownTOC levels="2,3" autolink="true" -->

- [Requirements](#requirements)
- [Role Variables](#role-variables)
  - [Service environment](#service-environment)
  - [Manual upgrade](#manual-upgrade)
  - [Manage keystore / trusts](#manage-keystore--trusts)
  - [Backup / restore](#backup--restore)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)

<!-- /MarkdownTOC -->

## Requirements

<!-- Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required. -->

## Role Variables

<!--  A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well. -->

### Service environment

```yaml
jira_service_environment:
  - LOG4J_FORMAT_MSG_NO_LOOKUPS=true
```

### Manual upgrade

For `confluence_manual_upgrade` see [jira](../jira). 

### Manage keystore / trusts

The list `confluence_trusts` can be used to import one or more CA bundles / certificates to facilitate secure communication. 

```yaml
confluence_trusts:
  - alias: gxd
    url: file:///vagrant/.ca/gxd/gxd.crt
```

This is based on the `trusts` attribute of [c2platform.core.java](https://github.com/c2platform/ansible-collection-core/tree/master/roles/java#manage-keystore--trusts) Ansible role.

### Backup / restore

Backup and restore is fully automated using **backup** role in the [c2platform.mgmt](https://github.com/c2platform/ansible-collection-mgmt) collection.

## Dependencies

<!--   A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles. -->

This role is dependent on the [c2platform.core](https://github.com/c2platform/ansible-collection-core) collection. For example it uses the roles.

* [lcm](../lcm)
* [postgresql_tasks](../postgresql_tasks)
* [java](../java)

## Example Playbook

<!--   Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too: -->

```yaml
    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }
```

