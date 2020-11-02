# ansible-lxdlxc-container-prototype

A prototype managing lxdlxc containers using ansible.
The basic idea is to ssh into the lxd host and
communicate with the container via the lxd command line
tools.

![infrastructure](doc/images/infra.png)

## Modifications to sshjail

### Variablen und Bezeichnungen

1. Name of the transport: `sshjail` -> `sshlxculi`
2. jailname -> containername
3. jailhost -> lxchost
4. jailspec -> containerspec
5. self.jid -> N/A
6. self.jname -> self.containername
7. self.connector -> N/A

### Methods

1. match_jail -> match_container
2. _jailhost_command ->_lxchost_command
3. get_jail_id -> get_lxc_name
4. get_jail_connector -> N/A
!! 5. get_jail_path -> N/A

### Commands

1. `jls -q jid name host.hostname path` -> `lxc list --columns n --format csv`

## Problems

### Separate pythons for lxchost and container

Currently, it seems that both the lxchost and the container have to use the same python, i.e. `/usr/bin/python3`.

### Platform unknown on host ubuntu-2004 is using the discovered Python interpreter at /usr/bin/python

Fixed by:

```diff
diff --git a/host_vars/ubuntu-2004.yml b/host_vars/ubuntu-2004.yml
index 88c385b..9f4417b 100644
--- a/host_vars/ubuntu-2004.yml
+++ b/host_vars/ubuntu-2004.yml
@@ -1,2 +1,3 @@
 ansible_connection: sshlxculi
 ansible_host: ubuntu-2004@hetzner-de
+ansible_python_interpreter: /usr/bin/python3
```

## Links

* [sshlxc](https://github.com/antifuchs/ansible-sshlxd-connection)
* [sshjail](https://github.com/austinhyde/ansible-sshjail)
* [sshjail - fork](https://github.com/seliopou/ansible-sshjail)
* [lxc_ssh](https://github.com/chifflier/ansible-lxc-ssh)
