Copyright(c) 2024 Intel Corporation

Released: July 26, 2024
Version: 1.14.1.0

Supported ESXi release: 8.0

===============================================================================

Contents:
========
- Overview
- Supported Features
- New Features
- Bug Fixes
- Known Issues and Workarounds
- Important Notes
- Installation
- Command Line Parameters
- Additional Notes
- Previously Released Versions

===============================================================================


Overview:
---------
Intel esxcli plug-in for managing 700 and 800 Series Intel(r) Ethernet Network Adapters.
Network adapters supported by Intel Ethernet drivers on ESXi 6.5, ESXi 6.7 and ESXi 7.0.

The extended esxcli tool supports following operations:

Forward Error Correction operations:
   - To set the FEC mode on a specified NIC

   esxcli intnet fec set [cmd options]

   Cmd options:
      -m|--mode            Set FEC mode to: Auto-FEC, No-FEC, RS-FEC, or FC-FEC/BASE-R (required).
      -n|--vmnic           Specifies the name of the vmnic name to operate on.
                           This must be one of the devices listed by the network nic list command (required).

   - To get FEC status on a specified NIC

   esxcli intnet fec get [cmd options]

   Cmd options:
      -n|--vmnic           Specifies the name of the vmnic to operate on.
                           This must be one of the devices listed by the network nic list command (required).

   - To list the FEC values for all supported vmnics

   esxcli intnet fec list

SR-IOV VF related operations:
   - To set/get a Virtual Function (VF) with the following command line format

   esxcli intnet sriovnic vf set [cmd options]

   Cmd options:
      -t|--trusted         Set trusted mode to true or false (required).
      -s|--spoofchk        Set mac/vlan spoof check to true or false (optional).
      -v|--vfid=<long>     Specifies the VF id for the virtual function.
                           This must be one of the VF IDs listed by the sriovNic vf list command (required).
      -n|--vmnic=<str>     Specifies the name for the SRIOV NIC.
                           This must be one of the devices listed by the sriovNic list command (required).

   - To list the current settings for a VF or for all VFs. The supported command line will be

   esxcli intnet sriovnic vf get [cmd options]

   Cmd options:
      -v|--vfid=<long>     Specifies VF id of the virtual function that is the subject of the information query (optional).
      -n|--vmnic=<str>     Specifies the name of the SRIOV NIC that is the subject of the information query.
                           This must be one of the devices listed by the sriovNic list command (required).

   If -v or --vf-id is not provided, the output will list existing settings for all VFs

Miscellaneous operations:
   - To Enable/Disable VMDQ VSIs to send loopback traffic on the given PF.

   esxcli intnet misc vmdqlb set [cmd option]

   Cmd options:
      -l|--loopback        Enable/Disable the VMDQ VSIs loopback (1 - enable (default), 0 - disable) (required).
      -n|--vmnic=<str>     Specifies the name for the vmnic to operate on.
                           This must be one of the devices listed listed by the network nic list command (required).

   - To get status of VMDQ VSIs loopback on the given PF.

   esxcli intnet misc vmdqlb get [cmd option]

   Cmd options:
      -n|--vmnic=<str>     Specifies the name for the vmnic to operate on.
                           This must be one of the devices listed by the network nic list command (required).

Link privileges operations:
   - To enable/disable link privileges for administrative operation on the link with the following command

   esxcli intnet admin link set [cmd options]

   Cmd options:
      -p|--priv=<str>      Enable/Disable link admin mode (required)
      -n|--vmnic=<str>     Specifies the name of the vmnic to operate on.
                           This must be one of the devices listed by the network nic list command (required).

   - To get the link privileges with the following command

   esxcli intnet admin link get [cmd options]

   Cmd options:
      -n|--vmnic=<str>     Specifies the name of the vmnic to operate on.
                           This must be one of the devices listed by the network nic list command (required).

   - To list the link privileges with the following command

   esxcli intnet admin link list

Optical Module Dump:
   - To get the optical Module dump

   esxcli intnet module read [cmd options]

   Cmd options:
   -n|--vmnic=<str>        Specifies the name of the vmnic to operate on.
                           This must be one of the devices listed by the network nic list command (required).

RSS operations:
   - To get RSS information with the following command

   esxcli intnet rss get [cmd options]

   Cmd options:
      -n|--vmnic=<str>     Specifies the name of the vmnic to operate on.
                           This must be one of the devices listed by the network nic list command (required).

DDP profiles operations:
   - To list all DDP profiles

   esxcli intnet ddp list

   - To load DDP profile

   esxcli intnet ddp load [cmd options]

   Cmd options:
   -p|--profile-name=<str>  DDP profile name (required).
                            DDP package should be located at: /store/intel/i40en/ddp.
   -n|--vmnic=<str>         vmnic name of the PF0 port (required).

   - To roll back recent DDP profile

   esxcli intnet ddp rollback [cmd options]

   Cmd options:
   -n|--vmnic=<str>        vmnic name of the PF0 port (required).

QinQ for VF:
   - To set the TPID on a specified NIC

   esxcli intnet qinq tpid set [cmd options]

   Cmd options:
   -s|--tpid=<str>         Specifies 0x8100 or 0x88a8 ethertype for outer tag (s-tag).
   -n|--vmnic=<str>        Specifies the name of the vmnic name to operate on.
                           This must be one of the devices listed by the network NIC list command (required).

   - To get current TPID on a specified NIC

   esxcli intnet qinq tpid get [cmd options]

   Cmd options:
   -n|--vmnic=<str>        Specifies the name of the vmnic name to operate on.
                           This must be one of the devices listed by the network NIC list command (required).

NIC temperature readout:
   - To read temperature sensor readings from a specified NIC in Celsius degrees

   esxcli intnet temperature get [cmd options]

   Cmd options:
   -n|--vmnic=<str>        Specifies the name of the vmnic name to operate on.
                           This must be one of the devices listed by the network NIC list command (required).

Dump FW debug clusters:

   esxcli intnet debug fw dump [cmd options]

   Cmd options:
   -c|--clusters=<str>   Clusters to be dumped. Comma separated list of cluster names.
                         Dumps all clusters if option not used.
   -l|--list             List cluster names.
   -n|--vmnic=<str>      Vmnic name to operate on (required).

Floating VEB:
   - To set Floating VEB on a specified VF

   esxcli intnet sriovnic vf set [cmd options]

   Cmd options:
   -f|--floating=<bool> Sets Floating VEB mode to true or false (required).
   -v|--vfid=<long>     Specifies VF id of the virtual function that is the subject of the setting (required).
   -n|--vmnic=<str>     Specifies the name of the SRIOV NIC that is the subject of the setting (required).
                        This must be one of the devices listed by the sriovNic list command.

   - To get Floating VEB on a specified VF
	
   esxcli intnet sriovnic vf get [cmd options]

   Cmd options:
   -v|--vfid=<long>     Specifies VF id of the virtual function that is the subject of the information query (optional).
                        If vfid is not provided the output will list existing settings for all VFs.
   -n|--vmnic=<str>     Specifies the name of the SRIOV NIC that is the subject of the information query (required).
                        This must be one of the devices listed by the sriovNic list command.

GNSS information:
   - To get GNSS module information

   esxcli intnet gnss get [cmd options]

   Cmd options:
   -n|--vmnic=<str>	   Vmnic name to operate on (required).


Supported Features:
-------------------
- Enable/Disable VF trust mode
- Enable/Disable VF spoof-check
- Query current settings for VFs
- Read/Modify FEC mode
- Enable/Disable VMDQ VSIs loopback
- Enable/Disable link privileges to operate on the link administratively
- Optical Module dump
- Get RSS information
- List/Load/Rollback DDP profiles
- Firmware Debug Dump
- Support for Outer VLAN TPID configuration
- QinQ for VF
- NIC temperature sensor readout
- Dump all FW Debug Clusters
- Floating VEB
- Get GNSS module information


New Features:
-------------
- Allow get VMDQ loopback command for i40en driver

Bug Fixes:
----------
- Added error message when no Intel NIC driver installed
- Make cluster names passed by user case insensitive

Known Issues and Workarounds:
-----------------------------
- None


Important Notes:
----------------
- Setting a VF is non-persistent
   It is required to set a VF to the desired operation every time after rebooting a VM or host since
   ESXi kernel may assign a different VF to the VM after reboot.

   Desired esxcli commands can be added to /etc/rc.local.d/local.sh file
   so that these commands can execute every time the host is power cycled or rebooted.
   For more information, please consult VMware KB Article # 2043564 - Modifying the rc.local or local.sh file
   in ESX/ESXi to execute commands while booting. https://kb.vmware.com/s/article/2043564

- Setting link privileges is non-persistent across reboots.

- Optical module detects the type of module (SPF/QSFP) and displays the module eeprom


Installation:
-------------
1. Install the vib file
   esxcli software vib install -v <full path>/vib
2. Reboot the ESXi host

"intnet" namespace in esxcli will be accessible to configure aforementioned features
if the plug-in install is successful.


Command Line Parameters:
------------------------
- Set VF 1 of vmnic0 as trusted
  esxcli intnet sriovnic vf set -t on -v 1 -n vmnicX

- Disable VF spoof-check for VF 1
  esxcli intnet sriovnic vf -v 1 -n vmnicX -s off

- Get current settings for VF 1
  esxcli intnet sriovnic vf get -n vmnicX -v 1

- Get current settings for all VFs
  esxcli intnet sriovnic vf get -n vmnicX

- Set the FEC Mode to auto
  esxcli intnet fec set -n vmnicX -m Auto-FEC

- Disable VMDQ VSIs loopback
  esxcli intnet misc vmdqlb set -l 0 -n vmnicX

- Enable VMDQ VSIs loopback
  esxcli intnet misc vmdqlb set -l 1 -n vmnicX

- Get VMDQ VSIs loopback
  esxcli intnet misc vmdqlb get -n vmnicX

- Enable link privileges on vmnic0
  esxcli intnet admin link set -p enable -n vmnicX

- Disable link privileges on vmnic0
  esxcli intnet admin link set -p disable -n vmnicX

- Get link privileges for vmnic0
  esxcli intnet admin link get -n vmnicX

- List link privileges for all vmnics
  esxcli intnet admin link list

- Enable FW LLDP agent
  esxcli intnet fwLldpAgent set -s enable -n vmnicX

- Disable FW LLDP agent
  esxcli intnet fwLldpAgent set -s disable -n vmnicX

- Get FW LLDP agent status
  esxcli intnet fwLldpAgent get -n vmnicX

- Dump optical module
  esxcli intnet module read -n vmnicX

- List all DDP profiles if any
  esxcli intnet ddp list

- Load DDP profile
  esxcli intnet ddp load -p geneve-pctype.pkg -n vmnicX

- Roll back recent DDP profile
  esxcli intnet ddp rollback -n vmnicX

- Read NIC temperature
  esxcli intnet temperature get -n vmnicX

- Enable/disable Floating VEB for VF Y
  esxcli intnet sriovnic vf set -f true/false -v Y -n vmnicX

- Get GNSS status
  esxcli intnet gnss get -n vmnicX


Additional Notes
----------------
- Trusted and spoof-check are boolean type. Therefore, the following parameter values are supported
  0, 1, n[o], y[es], f[alse], t[rue], off, on
- Trust and spoof-check features are independent, i.e. spoof-check can be disabled without enabling trusted mode
- Listing VFs and their VF ids is supported by the inbox esxcli command, i.e
     esxcli network sriovnic vf list -n vmnicX
- Find VMX Cartel ID displayed by esxcli network sriovnic vf list -n vmnicX
     esxcli vm process list
- Link privileges can be changed only when the link is up, set the link up/down using the following command
     esxcli network nic up/down -n vmnicX


=================================================================================

Previously Released Versions:
-----------------------------
- Intel esxcli Version: 1.13.2.0
   Supported ESXi release: 8.0
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
      - Enable/Disable link privileges to operate on the link administratively
      - Optical Module dump
      - Get RSS information
      - List/Load/Rollback DDP profiles
      - Firmware Debug Dump
      - Support for Outer VLAN TPID configuration
      - QinQ for VF
      - NIC temperature sensor readout
      - Dump all FW Debug Clusters
      - Floating VEB
      - Get GNSS module information
   Bug Fixes:
      - Removed warning message when DDP load was successful
   Known Issues:
      - None

- Intel esxcli Version: 1.12.5.0
   Supported ESXi release: 7.0, 8.0
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
      - Enable/Disable link privileges to operate on the link administratively
      - Optical Module dump
      - Get RSS information
      - List/Load/Rollback DDP profiles
      - Firmware Debug Dump
      - Support for Outer VLAN TPID configuration
      - QinQ for VF
      - NIC temperature sensor readout
      - Dump all FW Debug Clusters
   Bug Fixes:
      - Fixed displaying FEC values for supported NICs
      - Removed EETRACK_ID from debug dump file to make it compatible with DPDT tool
      - Fixed getting NIC temperature
   Known Issues:
      - None

- Intel esxcli Version: 1.11.1.0
   Supported ESXi release: 7.0, 8.0
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
      - Enable/Disable link privileges to operate on the link administratively
      - Optical Module dump
      - Get RSS information
      - List/Load/Rollback DDP profiles
      - Support for Outer VLAN TPID configuration
      - QinQ for VF
      - NIC temperature sensor readout
   Bug Fixes:
      - None
   Known Issues:
      - None

- Intel esxcli Version: 1.10.9.0
   Supported ESXi release: 7.0, 8.0
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
      - Enable/Disable link privileges to operate on the link administratively
      - Optical Module dump
      - Get RSS information
      - List/Load/Rollback DDP profiles
      - Support for Outer VLAN TPID configuration
      - QinQ for VF
   Bug Fixes:
      - None
   Known Issues:
      - None

- Intel esxcli Version: 1.9.2.0
   Supported ESXi release: 6.5, 6.7, 7.0
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
      - Enable/Disable link privileges to operate on the link administratively
      - Optical Module dump
      - Get RSS information
      - List/Load/Rollback DDP profiles
      - Support for Outer VLAN TPID configuration
   Bug Fixes:
      - Setting unsupported FEC mode will provide proper error message
   Known Issues:
      - None

- Intel esxcli Version: 1.6.5.0
   Supported ESXi release: 6.5, 6.7, 7.0
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
      - Enable/Disable link privileges to operate on the link administratively
      - Optical Module dump
      - Enable/Disable FW LLDP agent
      - List/Load/Rollback DDP profiles
   Bug Fixes:
      - None
   Known Issues:
      - None

- Intel esxcli Version: 1.5.2.0
   Supported ESXi release: 6.5 and 6.7
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
      - Enable/Disable link privileges to operate on the link administratively
      - Optical Module dump
      - Enable/Disable FW LLDP agent
   Bug Fixes:
      - Fix get/list FEC status of Requested FEC
   Known Issues:
      - None

- Intel esxcli Version: 1.4.1
   Supported ESXi release: 6.5 and 6.7
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
      - Enable/Disable link privileges to operate on the link administratively
      - Optical Module dump
   Bug Fixes:
      - Fix get/list FEC status of Requested FEC
   Known Issues:
      - None

- Intel esxcli Version: 1.2.1
   Supported ESXi release: 6.0, 6.5 and 6.7
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
      - Read/Modify FEC mode
      - Enable/Disable VMDQ VSIs loopback
   Bug Fixes:
      - None
   Known Issues:
      - Running command to get/list FEC status on vmnics with Requested FEC set to "Auto-FEC"
        will print Requested FEC as "RS-FEC"

- Intel esxcli Version: 1.0.2
   Supported ESXi release: 6.5 and 6.7
   Features Supported:
      - Enable/Disable VF trust mode
      - Enable/Disable VF spoof-check
      - Query current settings for VFs
   Bug Fixes:
      - None
   Known Issues:
      - None
