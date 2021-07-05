# This is a **WIP** role

# sap-netweaver-ha-pacemaker
HA configuration of pacemaker for SAP Netweaver software 

### Role variables

|                          variable                          |                                  info                                  |        required?        |
|:----------------------------------------------------------:|:----------------------------------------------------------------------:|:-----------------------:|
| sap_netweaver_ha_pacemaker_hacluster_manage_shared_storage |              Attempt to manage shared (eg. nfs) storage?               |   no, default is yes    |
|       sap_netweaver_ha_pacemaker_hacluster_node1_ip        |                  IP address of first node in cluster                   |           yes           |
|      sap_netweaver_ha_pacemaker_hacluster_node1_fqdn       |                     FWDN of first node in cluster                      |           yes           |
|       sap_netweaver_ha_pacemaker_hacluster_node2_ip        |                  IP address of second node in cluster                  |           yes           |
|      sap_netweaver_ha_pacemaker_hacluster_node2_fqdn       |                     FWDN of second node in cluster                     |           yes           |
|            sap_netweaver_ha_pacemaker_ensa_ver             |                            ENSA/ERS version                            |      no, default 2      |
|    sap_netweaver_ha_pacemaker_hacluster_manage_azure_lb    |              Default: no. Deal with Azure load balancer?               |           no            |
|               sap_netweaver_ha_pacemaker_sid               |                          SID of this instance                          |           yes           |
|          sap_netweaver_ha_pacemaker_profile_path           |                Full path of directory holding profiles                 | no, is generated sanely |
|       sap_netweaver_ha_pacemaker_instance_name_ascs        |                           ASCS instance name                           |           yes           |
|      sap_netweaver_ha_pacemaker_ascs_instance_number       |                          ASCS instance number                          |           yes           |
|        sap_netweaver_ha_pacemaker_ascs_profile_file        |                   Full path and name of ASCS profile                   | no, is generated sanely |
|        sap_netweaver_ha_pacemaker_ascs_profile_path        |              Full path to directory holding ASCS profile               | no, is generated sanely |
|            sap_netweaver_ha_pacemaker_ascs_vip             |             Virtual (cluster managed) IP address for ASCS              |           yes           |
|        sap_netweaver_ha_pacemaker_instance_name_ers        |                           ERS Instance name                            |           yes           |
|       sap_netweaver_ha_pacemaker_ers_instance_number       |                          ERS instance number                           |           yes           |
|        sap_netweaver_ha_pacemaker_ers_profile_file         |                Full path and file name of ASCS profile                 | no, is generated sanely |
|        sap_netweaver_ha_pacemaker_ers_profile_path         |              Full path to directory holding ASCS profile               | no, is generated sanely |
|             sap_netweaver_ha_pacemaker_ers_vip             |             Virtual (cluster managed) IP address for ASCS              |           yes           |
|        sap_netweaver_ha_pacemaker_e4s_repos_ppc64le        |                                                                        |           no            |
|        sap_netweaver_ha_pacemaker_e4s_repos_x86_64         |                                                                        |           no            |
|          sap_netweaver_ha_pacemaker_fsdevice_ascs          |                    ascs profile filesystem resource                    |           yes           |
|          sap_netweaver_ha_pacemaker_fsdevice_ers           |                    ers profile filesystem resource                     |           yes           |
|         sap_netweaver_ha_pacemaker_fsdevice_sapmnt         |                       sapmnt filesystem resource                       |           yes           |
|          sap_netweaver_ha_pacemaker_fsdevice_sys           |                        sys filesystem resource                         |           yes           |
|         sap_netweaver_ha_pacemaker_fsdevice_trans          |                       trans filesystem resource                        |           yes           |
|           sap_netweaver_ha_pacemaker_mntdir_ascs           |                 ASCS filesystem mount point directory                  |           yes           |
|           sap_netweaver_ha_pacemaker_mntdir_ers            |                  ERS filesystem mount point directory                  |           yes           |
|          sap_netweaver_ha_pacemaker_mntdir_sapmnt          |                sapmnt filesystem mount point directory                 |           yes           |
|           sap_netweaver_ha_pacemaker_mntdir_sys            |                  sys filesystem mount point directory                  |           yes           |
|          sap_netweaver_ha_pacemaker_mntdir_trans           |                 trans filesystem mount point directory                 |           yes           |
|             sap_netweaver_ha_pacemaker_fstype              | Type of filesystem resource of managed filesystems see: pcs fstype=xxx |           yes           |
|         sap_netweaver_ha_pacemaker_fstype_<thing>          |  Type of filesystem resource for <thing> (inhereted value from above)  |           no            |
|        sap_netweaver_ha_pacemaker_fsr_name_<thing>         |          PCS resource name for managed filesystem of <thing>           |           no            |

#### Filesystems
`sap_netweaver_ha_pacemaker_profile_path` is the root directory holding the "profiles" of ASCS and ERS. It is generated based on convention and the provided SID. `sap_netweaver_ha_pacemaker_ascs_profile_path` and `sap_netweaver_ha_pacemaker_ers_profile_path` are set to it, and the respective `profile_file` values are generated based on the particular instance names and IDs.


This role configures pcs to manage filesystems, which is to say they are mounted correctly (everywhere or only one node, as appropriate).    
`sap_netweaver_ha_pacemaker_fsdevice_ascs` is mounted at `sap_netweaver_ha_pacemaker_ascs_profile_path`    
`sap_netweaver_ha_pacemaker_fsdevice_ers` is mounted at `sap_netweaver_ha_pacemaker_ascs_profile_path`    
`sap_netweaver_ha_pacemaker_fsdevice_sapmnt` is mounted at `sap_netweaver_ha_pacemaker_ascs_profile_path`    
`sap_netweaver_ha_pacemaker_fsdevice_sys` is mounted at `sap_netweaver_ha_pacemaker_ascs_profile_path`    

### STONITH fencing
The following variables are common to all fencing devices (except those on hyperscalers)

|                         variable                         |                                      info                                       | required? |
|:--------------------------------------------------------:|:-------------------------------------------------------------------------------:|:---------:|
|      sap_netweaver_ha_pacemaker_fencing_device.name      |                           Name of the fencing device                            |    yes    |
|      sap_netweaver_ha_pacemaker_fencing_device.type      |                               Fencing device type                               |   yes *   |
|       sap_netweaver_ha_pacemaker_fencing_device.ip       |                        IP address of the fencing device                         |    yes    |
|      sap_netweaver_ha_pacemaker_fencing_device.user      |                    Username to connect to the fencing device                    |    yes    |
|      sap_netweaver_ha_pacemaker_fencing_device.pwd       |                    Password to connect to the fencing device                    |    yes    |
| sap_netweaver_ha_pacemaker_fencing_device.pcmk_host_list |                 List of nodes controlled by the fencing device                  |   yes *   |
| sap_netweaver_ha_pacemaker_fencing_device.password_file  | Full path/filename to 'password script' to use for fencing device configuration |    no     |
| sap_netweaver_ha_pacemaker_fencing_device.pcmk_host_map  |                    Mapping of the hostnames/ip of the nodes                     |   yes*    |
 
\* Must provide either a pcmk_host_list or a pcmk_host_map, per https://access.redhat.com/solutions/2619961 for a discussion on host_list vs host_map

This is a list of the most common fencing devices:
 - fence_vmware_soap - VMWare
 - fence_vmware_rest - VMWare
 - fence_ipmilan - IPMI
 - fence_ilo4_ssh - HP ILO
 - fence_rhevm - RHV
 - fence_azure_arm - Azure


 ## TODO and Notes
* Fencing should perhaps be extracted to own role to share with redhat_sap.sap-hana-ha-pacemaker
* Verify drivers other than fence_vmware_rest support passwd_script option
* Support >2 nodes
* Support >2 links
* This remains mostly untested against a range of systems.
* Var docs
* FS paths should match samples from docs
* Is this the right place for Azure LB?
* If using clustering, the sap_s4netweaver_deployment is likely going to need the hosts setup, so this is a dupe
* /etc/hosts perhaps should have ansible tags in it, either way, so the several roles can not clobber each other