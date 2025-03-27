## Hub Cluster installation on gen-9 hardware

### initial preparation 

#### Generate the ssh key for authenticating with core OS. 


1. Access to the BMC of the nodes. 

    `HW basic configurations or Settings :`

> Refer HW CERTIFICATION NOTE  to set bios settings for nodes . In NCES we are using compact Hub Cluster meaning 3 servers act as Master+Worker+ODF node hence . Bios Settings are done for master and storage node related.


    1a. Highlevel settings :
    *) Boot mode is set to UEFI.
    *) For worker nodes HW RAID1 is configured (as they supposed to have 2 OS disks)
    *) For master nodes RADI10 is configured for the 4 OS Disks. Extra capacity disks in the master nodes shall be used by the ODF as storage backend devices. 

Make sure you have gone through TSN-01 and TSN-03 ( available on TSN Section)  for this step as there might be little deviation from base artifacts or configuration approach . Do check Latest HW Certification Latest issue for particular HW to align with NCP release . 

2. DNS entries for the Openshift clusters is a must, otherwise the installation would fail.
3. Generate a keypair for ssh access of the HUB cluster nodes, using ssh-keygen. Preferred algorithm is `ed25519`. Use the command, It will keep pub in .ssh folder.


```
[root@ncputility ncp]# ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/root/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_ed25519
Your public key has been saved in /root/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:cPryPsjzimE7ks5xceRKXYFr5QlpGk00t5q01qrYxJU root@ncputility.nokiptchub01.nokia.usa
The key's randomart image is:
+--[ED25519 256]--+
|     ++oo        |
|    . *o.o       |
|     ++=+.       |
|    .=o@o        |
|    o.E S        |
|   o * o         |
|  ..B.o..        |
| .oB.=+o.        |
| .+.=..=+.       |
+----[SHA256]-----+
[root@ncputility ncp]# 
```


#### Preparing the artifacts

1. The used method is the Agent-based installation for the HUB cluster.
2. For the HUB cluster installation, the openshift-install binary will be used. It is important to know that for each OCP version a dedicated version of 3. 3. 3. openshift-install binary exists. In the ncp_tools.tar.gz, all the openshift related binaries are for OCP version 4.14.22.
4. There are two main files which needs to be prepared, the agent-config.yaml and the install-config.yaml. 
5. The install-config.yaml contains the cluster specific settings, like which CNI shall be used, cluster network, service network, base domain and cluster network, ssh-key. Furthermore, in this file the local mirror registry shall be specified as the source of the images. 
6. Required inputs for install_config can be checked or gathered like mentioned in Hub Cluster Install Config Inputs
7. In the agent-config.yaml, all the nodes shall be specified and their networking which will be part of the HUB cluster.
8. Required inputs for install_config can be checked or gathered like mentioned in Hub Cluster Agent Config Inputs
