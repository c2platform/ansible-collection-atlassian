# Ansible Role jira

Installs [Atlassian Jira](https://www.atlassian.com/software/jira/) on RedHat/CentOS or Ubuntu. Default Jira will be configured to be used via a reverse proxy server. So after the first / fresh provision navigate to Jira via the reverse proxy and complete the setup

<!-- MarkdownTOC levels="2,3" autolink="true" -->

- [Requirements](#requirements)
- [Role Variables](#role-variables)
  - [Service environment](#service-environment)
  - [Manage keystore](#manage-keystore)
  - [Manual upgrade](#manual-upgrade)
  - [jira_max_http_header_size](#jira_max_http_header_size)
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

### Manage keystore

The list `jira_trusts` can be used to import one or more CA bundles to facilitate secure communication.

```yaml
jira_trusts:
  - alias: gxd
    url: file:///vagrant/.ca/gxd/gxd.crt
```

This is based on the `trusts` attribute of [c2platform.core.java](https://github.com/c2platform/ansible-collection-core/tree/master/roles/java#manage-keystore--trusts) Ansible role.

### Manual upgrade

Use `jira_manual_upgrade` to perform an upgrade manually. Set to `yes` and provision. The role will prepare upgrade but not run the installer and start services. The installer will be downloaded to `/opt/jira` and a response file `/opt/jira/reponse.varfile`.

```bash
/opt/jira/atlassian-jira-software-8.5.5-x64.bin -q -varfile /opt/jira/response.varfile
```

Note: the application directory `/opt/jira/jira/app` is not configured when performing a manual upgrade. This is because the installer creates the application directory. To configure the application directory just run the jira play again.

### jira_max_http_header_size

When using SSO solution like Keycloak it is better / necessary to configure `maxHttpHeaderSize` in `server.xml` to higher value of `16834`. Otherwise you might get error messages like the one shown below. Of course, when running Confluence behind a reverse proxy, for example Apache2, you will also need to configure `LimitRequestFieldSize` to equal higher value.

> HTTP Status 400 – Bad Request
> 
> Type Exception Report
> 
> Message Request header is too large
> 
> Description The server cannot or will not process the request due to something that is perceived to be a client error (> e.g., malformed request syntax, invalid request message framing, or deceptive request routing).
> 
> Exception
> 
> java.lang.IllegalArgumentException: Request header is too large
> org.apache.coyote.http11.Http11InputBuffer.parseHeaders(Http11InputBuffer.java:594)
> org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:283)
> org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65)
> org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:868)
> org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1594)
> org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)
> java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
> java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
> org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
> java.base/java.lang.Thread.run(Unknown Source)
> 
> Note The full stack trace of the root cause is available in the server logs.
> Apache Tomcat/9.0.33

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




