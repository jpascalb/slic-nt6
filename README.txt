===============================================================================
OEM:SLP ACTIVATION GUIDE (DELL SLIC) FOR WINDOWS VISTA / 7 IN VIRTUALBOX
===============================================================================

Starting with Windows Vista and Windows 7 (SLP 2.X), OEM activation requires
the virtual BIOS to contain a structured ACPI table called SLIC, along with
a digital certificate (.xrm-ms) that matches that table.

-------------------------------------------------------------------------------
PREREQUISITES
-------------------------------------------------------------------------------
1. The Dell SLIC table binary file (e.g., "DELL_SLIC.bin"). You can extract
this from an actual physical Dell machine using tools such as RW-Everything.
2. The Dell OEM certificate file (e.g., "dell.xrm-ms").
3. A Dell OEM:SLP master product key corresponding to your exact version
of Windows (e.g., Windows 7 Professional OEM).
4. A virtual machine with Windows already installed.

-------------------------------------------------------------------------------
STEP 1: INJECTING THE ACPI SLIC TABLE INTO VIRTUALBOX (HOST MACHINE)
-------------------------------------------------------------------------------
1. Completely close the VirtualBox graphical interface.
2. Copy the "DELL_SLIC.bin" file to a known location on your host machine
(for example, "C:\VM\DELL_SLIC.bin"). 3. Open the command prompt (CMD) on your host machine and run the following
command to force VirtualBox to load the table into the virtual machine:

VBoxManage setextradata "Your_VM_Vista7" "VBoxInternal/Devices/acpi/0/Config/CustomTable" "C:\VM\DELL_SLIC.bin"

*(Note: Replace "Your_VM_Vista7" with the exact name of your virtual machine and
ensure that the path to the binary file is correct).*

-------------------------------------------------------------------------------
STEP 2: INSTALL THE DELL OEM CERTIFICATE (INSIDE THE VM)
-------------------------------------------------------------------------------
1. Start the virtual machine in Normal Mode.
2. Copy the certificate file "dell.xrm-ms" onto the virtual machine's disk
(for example, to the root directory "C:\").
3. Open the command prompt (CMD) inside the virtual machine with
**Administrator Privileges** (Right-click -> Run as Administrator).
4. Run the following command to install the certificate into the Windows
license store:

slmgr.vbs -ilc C:\dell.xrm-ms

5. Wait a few seconds until a pop-up window appears, indicating that
the certificate has been installed successfully. -------------------------------------------------------------------------------
STEP 3: ENTER THE OEM:SLP MASTER KEY
-------------------------------------------------------------------------------
In the same command console (running with Administrator privileges) inside the VM,
enter the Dell master key by executing:

slmgr.vbs -ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX

*(Replace the Xs with your Dell OEM:SLP key).*

-------------------------------------------------------------------------------
STEP 4: VERIFY ACTIVATION
-------------------------------------------------------------------------------
To confirm that the operating system has correctly validated the virtual SLIC
table and the certificate, run the following in the console:

slmgr.vbs -dli

A window will appear, which should state: "License Status: Licensed".

===============================================================================
IMPORTANT NOTE FOR WINDOWS 8 / 10 / 11 (SLP 3.0)
===============================================================================
Starting with Windows 8, Microsoft eliminated common OEM master keys. Each
physical Dell computer comes with a unique key, injected at the factory into an
ACPI table called "MSDM".

If you wish to activate a modern version of Windows 10/11 in VirtualBox using the
OEM license from your own physical host machine, you can configure VirtualBox to
read the BIOS tables from your actual PC and pass them to the virtual machine by
executing this command on the host (with the VM powered off):

VBoxManage setextradata "Your_Win10_VM" "VBoxInternal/Devices/acpi/0/Config/PassThroughTable" "true"

This will allow the VM to inherit the digital key from your actual motherboard.
===============================================================================
