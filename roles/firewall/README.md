# Role firewall (nftables)

## Purpose

Providing an inheriting nftables firewall for different roles.

## Inheritance

To open ports on your server you have to add a file in the name of your service. Take a look at the ssh configuration.
Unfortunately adding new rules into an existing setup only works using the nft command syntax.
Make sure to add the chain with the rules before adding the jump rule to the standard input chain.

If you have any questions about writing rules for nftables ask Lukas or Thore for help.

## Add a firewall for your role

To add a firewall for your role, don't do it in the firewall role itself. Instead create your own nftables ruleset (see above or ask for help) in your role definition. They should be a single file called ``$SERVICE.nftables`` and hand over the path to the file with the following definition in the ``meta/main.yml``:
```
dependencies:
  - { role: apply-firewall, role_firewall_config: "roles/$ROLE/files/$SERVICE.nftables", when: ansible_distribution_release == "stretch"  }

```
It is also possible to use a template instead of a file.
The meta-include will also take care of installing nftables on the host and enabling your configuration.
