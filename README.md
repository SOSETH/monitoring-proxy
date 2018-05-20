# monitoring-proxy

Re-expose monitoring endpoints (only prometheus for the moment) so that they are reachable from the master nodes even if they're not on the same network. The proxy re-encrypts the connections so that the client certificate of a connecting node can be verified which means that only authorized connections are forwarded, protecting the nodes.

## Dependencies
This role depends on the following SOSETH ansible roles:
* haproxy
* (optionally) local-ca

## Supported distributions
* Debian 9 (strech)

## Configuration
|Var|Default value|Description|
|---|-------------|-----------|
|`mon_forward_pki_enabled`|`True`|Whether to use the local-ca role to automatically generate the required certificate|
|`mon_forward_pki_name`|`prometheus`|If using local-ca, how is the CA this role is to use called?|
|`mon_forward_hostname`|`{{ ansible_fqdn }}`|This host's hostname as seen from the master|
|`mon_forward_items`|`[]`|What is this host supposed to forward(see below)|

`mon_forward_items` is a list of dicts with the following structure:
```
mon_forward_items:
  <fqdn of target host>:
    dest: <ip of target host>
    ports:
      - dest: <port to forward to>
        src:  <port to expose>
```
In the above example, `<port to expose>` would be forwarded to `<ip of target host>:<port to forward to>`.
