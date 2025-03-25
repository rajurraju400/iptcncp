> This project is created to provide complete step by step instruction to build the NCP/OCP hub cluster and deploy the workload cluster via the ZTP process. 

# NCP 24.7 MP1 installation on the HPE protait gen9 blade servers

> Procedure created for NAM Market's Tuchuong team members only. 


## Discliamer 

> Since it's old HW, can be used only for educational propuse only. 

#### NCP LLD for HUB and CWL clusters


#### IPMI and blade system role information

> "automation 0" blade server should not be used for any Scale-in/out or replacement or initial deployment. this automation server does not have any replication, so dont touch this server.


| Hardware role              | IPMI IP                    | Usr/Pass                   | External IP         | Usr/Pass  |        
| -------------------------- | -------------------------- | -------------------------- | ------------------- | --------- |
| Master 0                   | 135.104.149.28             | hp/password                |   135.104.149.142, 135.104.149.140 (VIP) | root/root |
| Master 1                   | 135.104.149.30             | hp/password                | 135.104.149.143 | root/root |
| Master 2                   | 135.104.149.31             | hp/password                | 135.104.149.144 | root/root |
| Worker 0                   | 135.104.149.32             | hp/password                |                 | root/root |
| Worker 1                   | 135.104.149.33             | hp/password                |                 | root/root |
| Edge 0                     | 135.104.149.34             | hp/password                | 135.104.149.145(reserve only) | root/root |
| automation 0               | 135.104.149.35             | hp/password                | 135.104.149.141 | cbis-admin/root |

> "automation 0" blade server should not be used for any Scale-in/out or replacement or initial deployment. this automation server does not have any replication, so dont touch this server.