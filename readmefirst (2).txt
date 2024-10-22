This release includes ixgbe Linux* Base Drivers for Intel(R) Ethernet Network
Connections.

The ixgbe driver supports devices based on the following controllers:
  * Intel(R) Ethernet Controller 82598
  * Intel(R) Ethernet Controller 82599
  * Intel(R) Ethernet Controller X520
  * Intel(R) Ethernet Controller X540
  * Intel(R) Ethernet Controller x550
  * Intel(R) Ethernet Controller X552
  * Intel(R) Ethernet Controller X553
- The ixgbevf driver can only be activated on kernels that support SR-IOV. It
  supports all of the above devices except for the Intel(R) Ethernet
  Controller 82598.

ixgbe-x.x.x.tar.gz
ixgbevf-x.x.x.tar.gz

Due to the continuous development of the Linux kernel, the drivers are updated
more often than the bundled releases. The latest driver can be found on
http://e1000.sourceforge.net (and also on http://downloadcenter.intel.com).

This release includes RPM packages that contain:
* Device driver signed with Intel's private key in precompiled kernel module
form
* RDMA driver
* Complete source code for above drivers
* Intel's public key

This release includes the Intel public key to allow you to authenticate the
signed driver in secure boot mode. To authenticate the signed driver, you must
place Intel's public key in the UEFI Secure Boot key database.

NOTES:
- The driver kernel module for a specific kernel version can be used with
errata kernels within the same minor OS version, unless the errata kernel broke
kABI. Whenever you update your kernel with an errata kernel, you must reinstall
the driver RPM package.
- The RDMA driver will be installed if you reinstall the driver RPM package. If
you want to remove the RDMA driver, you will have to do so every time you
install the RPM package (for example, when you update your kernel with an
errata kernel).
- If you decide to recompile the .ko module from the provided source files, the
new .ko module will not be signed with any key. To use this .ko module in
Secure Boot mode, you must sign it yourself with your own private key and add
your public key to the UEFI Secure Boot key database.


