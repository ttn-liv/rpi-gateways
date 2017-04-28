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

![](images/etcher.gif)

Prepare a fresh Raspberry Pi B 2+ iC880a, as per the build instructions. Then:
 1. flash a
fresh copy of Raspian-Lite (currently 2017-03-02) to a micro SD card
([etcher.io](https://etcher.io) offers a good, cross-platform solution),
 1. mount the newly flashed drive,
 1. add an empty file named `ssh` to the `boot` partition to allow ssh access
 to the pi,
 1. open `boot/config.txt` and remove the leading `#` on the line reading
 `dtparam=spi=on` to activate the SPI kernel module (the iC880a connects via SPI),
 1. cleanly unmount the drive and remove from your computer,
 1. insert the micro SD card into the RPi,
 1. connect a network cable to the RPi and power on,
 1. wait for a few minutes to allow the initial setup to run; the pi will
 auto-expand the file system, arrange the ssh daemon and reboot it's self,
 1. try and manually ssh to the pi (`ssh pi@raspberrypi.local` passsword: `rasbian`)
   * the pi must be the only `raspberrypi.local` device on the network(!),
   otherwise the ansible playbook might affect the wrong RPi,
   * if you get a warning about possible `WARNING: POSSIBLE DNS SPOOFING DETECTED!`,
   this it because you have connecting to another RPi called `raspberrypi.local`
   before, and this one has different ssh keys (it should). Run the suggested
   command: `ssh-keygen -f "~/.ssh/known_hosts" -R raspberrypi.local` to wipe
   the old key from your known_hosts list.
1. once you've verified the ssh connection manually (and accepted the new ECDSA key fingerprint — required to use Ansible to connect), run the Ansible playbook for
provisioning (below).

The provisoning playbook can be run interactively (where the terminal will
ask for each unknown) or the variables can be passed directly on the command line.

To run interactively:

```
ansible-playbook -i raspbian provisioning.yml
```

You will be asked to provide:

 * a unique alias to generate the hostname from,
 * a unique ssh port to connect via the jump host,
 * a unique ssh port for the jump host to forward prometheus stats,
 * a latitude for the gateway,
 * a longitude for the gateway location,
 * an altitude for the gateway location,
 * a github username to auto-dowload authorised ssh keys.

To provide all the variables up-front:

```
ansible-playbook -i raspbian provisioning.yml --extra-vars "new_hostname=<alias> jump_ssh_port=<6000-6999> prometheus_jump_port=<9000-9999> gate_lat=<latitude> gate_lon=<longitude> gate_alt=<altitude> github_user=<username>"
```

Once the playbook has run through, you will now be able to connect to the newly
rebooted RPi at `hostname-ttn.local`, without requiring a password. Additionally
you will notice that a new file with the gateway details has been created in the
configs directory (you should place this directory under version control), and
the file has been symlinked into the `host_vars` directory, and the alias
has been added to the `staging` inventory file.

## Staging

EITHER:

Add the hostname to the `staging` list if not done automatically as part of
the guided provisioning process.

OR:

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
staging list or use `--limit <alias>` to only deploy to a single host.

Once complete, record the reported `gateway_eui` (so it doesn't change with
hardware) and `tor_address`. You should now be able to make a direct ssh
connection to <ansible_user>@<tor_address> if your ssh client is set to
forward `.onion` addresses via tor.

Update the `ansible_host` and `ansible_port` to match the `control_hostname` and
`ssh_remote_port` respectively, so subsequent connections go via the jump host.

## production

Transfer any gateway names from staging to production.

Run `ansible-playbook -i production site.yml` to sequentially update each host.
