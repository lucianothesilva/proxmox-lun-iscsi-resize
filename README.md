
# How to resize a LUN iSCSI in Proxmox 

This process assumes you have already set up your Proxmox cluster with multipath and have already set up the resize in your SAN.

Display information about all physical volumes (PVs) with additional tag details, your LUN_name will be in /dev/mapper/Your_LUN_name. It will also show your device identifier.

    pvs -o +tags 

Show detailed information about the specified multipath device 'Your_LUN_name'.

    multipath -ll Your_LUN_name

List information about all available block devices in a tree-like format, so you can check the volume size.

    lsblk 

Trigger a rescan of the SCSI device 'sdX' (Inform your device identifier) to detect any changes (e.g., increased LUN size).

    echo 1 >/sys/block/sdX/device/rescan

Reload the multipath configuration to apply any changes detected during the rescan.

    multipath -r

Show updated detailed information about the specified multipath device 'LUN_name'.

    multipath -ll Your_LUN_name

Resize the physical volume to use any additional space detected on the multipath device.

    pvresize /dev/mapper/Your_LUN_name

After this the it should be automatically updated in Proxmox.
