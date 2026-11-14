# Upgrading nodes

## Control Plane and Workers

- Drain the node
- Replace the source parameter as needed, it can be to a custom image as well
  `kairos-agent upgrade --source oci://quay.io/kairos/hadron:v0.6.3-standard-amd64-generic-v4.4.0-k3s-v1.37.1-k3s1`

_Current fleet channel: Hadron v4.4.0, k3s v1.37.1+k3s1 (see auroraboot-config.yaml
and kairos-channel.yaml in fleet-infra). Rolling upgrades must step through every
intermediate k3s minor, one node at a time, starting with minis-32c6._

_The node will reboot_

- Uncordon the node once it is `Ready`
- Rerun the previous command but with `--recovery` if everything you test on
  the node looks good. The node will not reboot.

## Recovery partition

Run after the active partition is settled:

`kairos-agent upgrade --recovery --source oci://quay.io/kairos/hadron:v0.6.3-standard-amd64-generic-v4.4.0-k3s-v1.37.1-k3s1`

All three nodes had their recovery partitions refreshed to the v4.4.0 Hadron image
on 2026-11-14. The kairos-operator's NodeOpUpgrade jobs currently crash on these
nodes (the job uses `--source dir:/` and the agent's size walk dies on the
kernel's `/sys/module/realtek/drivers` readdir EIO) — use the manual command
until that is fixed upstream.
