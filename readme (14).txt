Ethernet Cmdlets for Intel(R) Ethernet
======================================

August 21, 2023


Contents
--------
- Overview
- Prerequisites
- Installation
- Help
- Debug Dump
- Firmware Log Usage
- Recommended Configurations for Firmware Logs
- DCB Status Usage
- Performance Profile Usage


Overview
========
This download contains Ethernet cmdlets for Microsoft* Windows* for PowerShell*, which allow you to get information, configure settings, or debug issues on supported Intel(R) Ethernet devices. Not all hardware and operating systems support all cmdlets.

- Ethernet cmdlets are supported on Intel(R) Ethernet 700 Series and newer devices.
- Firmware logging is supported only on Intel(R) Ethernet 800 Series devices.
- Debug dump is supported only on Intel(R) Ethernet 810 Series devices.


Firmware Logging
----------------
In Windows, you use PowerShell and Ethernet cmdlets to set the configuration for firmware logs and generate the logs.

Firmware logs are generated in a binary format and have to be decoded by Customer Support. Information collected is related only to firmware and hardware for debug purposes.


Prerequisites
=============
You must have the latest base driver and NVM installed.


Installation
============
For most Intel Ethernet devices:

1. Download the zip file with the Ethernet cmdlets from the Intel Download Center.
2. Save the zip file to the system you are trying to debug.
3. Extract files from the zip file.
4. In the extracted files, double-click on Ethernet_Cmdlets.exe.

The cmdlets will automatically install to the current user's PowerShell module directory (for example, to C:\Users\Administrator\Documents\WindowsPowerShell\Modules\IntelEthernetCmdlets).

After installation, you can use the cmdlets without manually importing the module.

NOTE: Installation steps may vary for some devices. Refer to the Intel(R) Ethernet Adapters and Devices User Guide for more information.


Help
====
For detailed usage information of the cmdlets available in this download, type the following at the PowerShell prompt:

  PS C:\> get-help IntelEthernetCmdlets

To get information on a specific cmdlet, type the following at the PowerShell prompt:

  PS C:\> get-help <cmdlet_name> -full

For example:

  PS C:\> get-help Get-IntelEthernet -full

NOTE: To show only examples, type:

  PS C:\> get-help <cmdlet_name> -examples


Debug Dump
==========
Debug dump uses an Ethernet cmdlet, Write-IntelEthernetDebugDump, to obtain runtime register values from the firmware and then write the results to a single file, for debugging complicated issues in the field. Debug dump captures more static information than firmware logs, and is a stateless snapshot of the whole device.

Like with firmware logs, the contents of the debug dump are not human-readable. You must work with Customer Support to decode the file.

Refer to the cmdlet help for more information.


Firmware Log Usage
==================
To capture a firmware log, you must do the following:
- Set the configuration for the firmware log.
- Capture the firmware log.
- Work with Customer Support to debug your issue.

NOTE:
- Both the device and the driver need to support firmware logging for the functionality to work. If you are not able to set the configuration and the problem persists, reinstall the driver.
- Firmware logging events and levels are device-wide settings. Changing the log configuration on one port will apply it to all ports on a device.


Setting the configuration of firmware logs
------------------------------------------
Use the Set-IntelEthernetLogConfig cmdlet provided in this download to change the configuration of firmware logs.

You can change the verbosity level of the logs for different categories of events (called "modules") or for groups of modules. You can set only one log level per module or per configuration group. Refer to the "Recommended Configurations for Firmware Logs" section later in this README for additional information.

To view the available modules for firmware logs, run the following in PowerShell and look at the list of available modules returned:

	PS C:\> Get-IntelEthernetLogConfig

Use the Reset-IntelEthernetLogConfig cmdlet to reset all modules to their default level, as determined by driver and device defaults. Use the Disable-IntelEthernetLogConfig to set logging for all modules to disabled.

Refer to the full cmdlet help in PowerShell for more information.


Generating a firmware log
-------------------------
To generate a firmware log for your issue, do the following:

1. In PowerShell, use the Set-IntelEthernetLogConfig cmdlet to set the logging level for your desired module(s). For example:

	PS C:\> Set-IntelEthernetLogConfig -Name "Intel(R) Ethernet Network Adapter E810-C-Q2" -Module "Link Management" -Level "Verbose"

2. In PowerShell, use the Start-IntelEthernetLog cmdlet to start collecting data and output it to a log file. For example:

	PS C:\> Start-IntelEthernetLog -Name "Intel(R) Ethernet Network Adapter E810-C-Q2"

NOTE:
- The driver must be running and enabled. The system will not start capturing firmware logs until you issue the PowerShell cmdlet above.
- You must enclose strings containing spaces, such as the file path, in quotation marks.

3. Perform the event that would cause the logging event.

4. From the PowerShell prompt, use the following to stop the logging session:

	PS C:\> Stop-IntelEthernetLog

5. Contact Customer Support for your device, for assistance with filing an issue. Provide driver information and attach the logs to your issue.


Recommended Configurations for Firmware Logs
============================================
The NVM sets default verbosity levels for different modules. In general, set the logging level to Verbose only for the configuration group(s) or module(s) you need to debug. Setting too many modules to Verbose can overrun the buffer.

The table below shows possible configuration groups or modules to set, depending on the type of issue you are trying to debug. If you see errors or suspect the issue could fall into the below categories, setting these configuration groups or individual modules will provide more information in the firmware log.

The left column shows possible -Configuration values to enter in the Set-IntelEthernetLogConfig cmdlet; the right column shows -Module values. Setting the -Configuration value will change the corresponding modules in the right column to the same log level. For example, setting the -Configuration value to "NVM" will automatically change the log level for the NVM, Authentication, and VPD modules. NOTE: You cannot specify both the -Configuration and -Module parameters.

Configuration Group             Corresponding Modules
===================             =====================
Initialization                  Control
--------------------------------------------------------
NVM                             NVM
                                Authentication
                                VPD
--------------------------------------------------------
IO                              I2C
                                SDP
                                MDIO
--------------------------------------------------------
Link Management                 Link Management
                                Link Control Technology
                                Link Topology Detection
--------------------------------------------------------
Rx                              Parser
                                Switch
                                ACL
                                Post
--------------------------------------------------------
Tx                              Scheduler
                                Tx Queue Management
--------------------------------------------------------
AQ Interface                    Admin Queue
                                HDMA
--------------------------------------------------------
Manageability                   Manageability
--------------------------------------------------------
Protocols                       LLDP
                                DCBx
--------------------------------------------------------
Infrastructure                  Watchdog
                                Task Dispatcher
                                General
                                IOSF
                                PF Registration
                                Module Versions
--------------------------------------------------------
XLR                             XLR
--------------------------------------------------------
QoS                             DCB
--------------------------------------------------------
Diagnostics                     SyncE
                                Health
--------------------------------------------------------
TimeSync                        Time Sync

========================================================


DCB Status Usage
================
Use the Get-IntelEthernetDCBStatus to output Data Center Bridging (DCB) Status information for an adapter. This cmdlet is intended to debug DCB issues.

The output of the Get-IntelEthernetDCBStatus cmdlet is a snapshot of the DCB state, and is formatted for outputting to a file.


Get-IntelEthernetDCBStatus Output Definitions
---------------------------------------------
Get-IntelEthernetDCBStatus outputs the following information:

DCBx - Data Center Bridging Exchange string that can contain these values:
  Not Started
  In Progress
  Done
  Multiple Peers
  Disabled

DCB status - Whether device is capable of DCB, and if DCB is enabled

Configuration Types:
  LocalConfig
  RemoteConfig

Each Configuration is an object that holds the following information:
  ETSCfg  - Operational DCB object
  ETSRec  - Desired DCB object

  Information within DCB objects:
    Willing - Determines whether this DCB will assume configuration of other DCB upon negotiation
    CBS     - Whether the Credit Based Shaper Transmission Selection Algorithm is supported
    MaxTCs  - The maximum number of traffic classes

    Traffic Class (TC) - Display information for each type of traffic
    Each TC has PFCEnabled, BW %, TSA, and UP bytes associated with it
      PFC Enabled - Whether Priority Flow Control is enabled for the TC
      BW %        - Bandwidth Percentage
      TSA         - Transmission Selection Algorithm
      UP          - User Priority

  PFC - Priority Flow Control information
    Willing   - Inherits PFC Configuration from peers
    PFCCap    - Maximum capacity of TCs that can support PFC
    PFCEnable - Whether PFC is enabled on the priority

  Apps:
    NumApps - Number of applications displayed

The cmdlet also outputs a list of User Priority TCs and lists PFC counters associated with each.

To see more detailed information and get access to the individual bytes contained in arrays, view the properties of the objects that Get-IntelEthernetDCBStatus outputs.
Each object has properties that can be referenced to provide access to this data.


Generating a DCB Status Log
---------------------------
To generate a DCB Status log for your issue, do the following:

1. In PowerShell, use the Get-IntelEthernetDCBStatus cmdlet to output the status to a file. For example:

  PS C:\> Get-IntelEthernetDCBStatus | fl | Out-File *path*\DCB_Log.txt

2. Contact Customer Support for your device, for assistance with filing an issue. Provide driver information and attach the output file to your issue.


Performance Profile Usage
=========================
The Performance Profile setting allows you to set a group of recommended settings for your use case. Setting a profile sets multiple Advanced Property settings that correspond to the profile.

Use the Get-IntelEthernetProfileSetting cmdlet to display a table of settings and their values for the specified Ethernet device and profile combination.


Setting the Performance Profile setting
---------------------------------------
1. Get a collection of adapters:

    PS C:\> $Adapters = Get-IntelEthernet

2. Observe what settings will change when a profile is set:

    PS C:\> Get-IntelEthernetProfileSetting -Adapter $Adapters[1] -ProfileName "Standard Server"

3. If the settings that will be changed are acceptable, apply the profile:

    PS C:\> Set-IntelEthernetSetting -Adapter $Adapters[1] -DisplayName "Performance Profile" -DisplayValue "Standard Server"


Legal / Disclaimers
===================

Copyright (C) 2021 Intel Corporation

This software and the related documents are Intel copyrighted materials, and your use of them is governed by the express license under which they were provided to you ("License"). Unless the License provides otherwise, you may not use, modify, copy, publish, distribute, disclose or transmit this software or the related documents without Intel's prior written permission.

This software and the related documents are provided as is, with no express or implied warranties, other than those that are expressly stated in the License.

Intel, the Intel logo, and other Intel marks are trademarks of Intel Corporation or its subsidiaries. Other names and brands may be claimed as the property of others.
