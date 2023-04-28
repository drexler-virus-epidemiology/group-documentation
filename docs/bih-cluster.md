# BIH HPC cluster usage

!!! note "Future content"
    This site is dedicated to the usage of the BIH HPC cluster. Generally, the
    [official documentation](https://bihealth.github.io/bih-cluster/) should be
    preferred for this.

## Access

The simplest way to access the cluster is via the [BIH-CUBI HPC OnDemand
Portal](https://hpc-portal.cubi.bihealth.org) using your regular Charité login
credentials.

The recommended way to access the cluster is via SSH. Extensive documentation
on how to create and configure an SSH key-pair is available in the [cluster
documentation](https://bihealth.github.io/bih-cluster/misc/ssh-basics/), with
specific instruction for Windows available under the chapter `Connecting`,
starting
[here](https://bihealth.github.io/bih-cluster/connecting/generate-key/windows/).

By default, the access is only granted from within the Charité. Acess via the
VPN needs to be requested separately, as described
[here](https://bihealth.github.io/bih-cluster/connecting/from-external) by
submitting the `VPN-Zusatzantrag B`.

## Configuration

On Unix systems, the access can be simply configured using the SSH main
configuration for each user located at `~/.ssh/config`. By adding a custom
configuration for the cluster, the access can be made a little more convenient.

```
Host *.cubi.bihealth.org
  ForwardAgent yes
  RequestTTY yes
  ServerAliveInterval 15
  ServerAliveCountMax 3
  IdentitiesOnly yes
  User cabe12_c # replace this with your HPC user name
  IdentityFile ~/.ssh/id_ed25519 # replace this with your SSH key
```

## File system overview

The OnDemand portal provides an
[overview](https://hpc-portal.cubi.bihealth.org/pun/sys/ood-bih-quotas) of
available storage for each user and the group.

## Partition overview

The nodes on the cluster are attributed to specific partitions based on their
use case. The details are described
[here](https://docs.hpc.bihealth.org/overview/job-scheduler/).

## TMUX

* show my tmux.conf

## RStudio on the cluster

The OnDemand platform allows for 
