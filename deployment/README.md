# TTN Gateway Deployment

These configuration files will provision and deploy TTN gateways using
Raspberry Pi and IMST iC880a condensor board. ssh access is provided by
making sure an outgoing ssh tunnel is maintained to a jump-host and grafana
server for monitoring.

Gateway setup takes 3 stages:

 * provisioning — Initially, create the users, ssh keys and install the software from a
 base Raspbian install,
 * staging — functional gateways connected to a local network,
 * deployment — deployed gateways in remote locations, with only remote access.

Deployment requires [ansible](https://www.ansible.com/) (tested on 2.2.1.0), where running
`ansible-playbook` with specific options, will run or update each gateway group.

Initially create a few files to hold the gateway inventory:

 * `group_vars/gateways.yml` from `group_vars/gateways.yml.example`,
 * `staging` from `staging.sample`,
 * `production` from `production.sample`,

## Provisioning

TODO: in progress.

Add the hostname to the `staging` list.

## Staging

Create `host_vars/<name>.yml` from `host_vars/example.yml` for the newly
provisioned device. Use the local hostname (`<name>.local`) for the
`ansible_host`. `ansible_port` should still be `22` for default ssh port.

Set the gateway specific values:

  * `ssh_remote_port` should be the unique remote ssh access port for the
  jump-host,
 * `prometheus_remote_port` is the unique port for the prometheus reporting,
 * `gateway_name` as you would like it to appear on TTN,
 * `gateway_email`, this can optionally be set for all gateways in
 `group_vars/gateways.yml`,
 * `gateway_lat`, `gateway_lon` & `gateway_alt` to set location.

Run `ansible-playbook -i staging site.yml` to deploy to all hosts in the
staging list.

Once complete, record the reported `gateway_eui` (so it doesn't change with
hardware) and `tor_address`. You should now be able to make a direct ssh
connection to <ansible_user>@<tor_address> if your ssh client is set to
forward `.onion` addresses via tor.

Update the `ansible_host` and `ansible_port` to match the `control_hostname` and
`ssh_remote_port` respectively, so subsequent connections go via the jump host.

## production

Transfer any gateway names from staging to production.

Run `ansible-playbook -i production site.yml` to sequentially update each host.
