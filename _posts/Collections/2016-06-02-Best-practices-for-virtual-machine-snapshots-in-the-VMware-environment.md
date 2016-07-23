---
layout: post
title: "Best practices for virtual machine snapshots in the VMware environment"
description: ""
category: Virtualization
tags:  [Snapshot]
---
{% include JB/setup %}

# Summary
+ Snapshots are not backups, the vm is running on the most current snapshot.
+ Snapshots only copies the delta disks, current state of the vm = original disk files + change log in the snapshot files
+ Size of the delta files will grow to the same size of the original base disk file
+ Best practice: only 2-3 snapshots in a chain and not last more than 24-72 hours.

You can also find the original artical below or [Right Here](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1025279)

# Purpose
This article provides best practice information for snapshots. It also provides links to resources that help you understand snapshots and troubleshoot snapshot issues.
Resolution

# Best practices
Snapshots are not backups. A snapshot file is only a change log of the original virtual disk. Therefore, do not rely on it as a direct backup process. The virtual machine is running on the most current snapshot, not the original vmdk disk files.

Snapshots are not complete copies of the original vmdk disk files. Taking a snapshot does not create a complete copy of the original vmdk disk file, rather it only copies the delta disks. The change log in the snapshot file combines with the original disk files to make up the current state of the virtual machine. If the base disks are deleted, the snapshot files are useless.

Delta files can grow to the same size as the original base disk file, which is why the provisioned storage size of a virtual machine increases by an amount up to the original size of the virtual machine multiplied by the number of snapshots on the virtual machine.

The maximum supported amount of snapshots in a chain is 32. However, VMware recommends that you use only 2-3 snapshots in a chain.

Use no single snapshot for more than 24-72 hours. Snapshots should not be maintained over long periods of time for application or Virtual Machine version control purposes.

+ This prevents snapshots from growing so large as to cause issues when deleting/committing them to the original virtual machine disks. Take the snapshot, make the changes to the virtual machine, and delete/commit the snapshot as soon as you have verified the proper working state of the virtual machine.

+ Be especially diligent with snapshot use on high-transaction virtual machines such as email and database servers. These snapshots can very quickly grow in size, filling datastore space. Commit snapshots on these virtual machines as soon as you have verified the proper working state of the process you are testing.

If using a third party product that takes advantage of snapshots (such as virtual machine backup software), regularly monitor systems configured for backups to ensure that no snapshots remain active for extensive periods of time.

+ Snapshots should only be present for the duration of the backup process.
+ Snapshots taken by third party software (called via API) may not show up in the vCenter Snapshot Manager. Routinely check for snapshots through the command-line.

An excessive number of delta files in a chain (caused by an excessive number of snapshots) or large delta files may cause decreased virtual machine and host performance.

Configure automated vCenter Server alarms to trigger when a virtual machine is running from snapshots. For more information, see [Configuring VMware vCenter Server to send alarms when virtual machines are running from snapshots (1018029)](http://kb.vmware.com/kb/1018029).

If hosts and/or vCenter Server are prior to vSphere 5.0 confirm that there are no snapshots present (through command line) before a Storage vMotion. If snapshots are present in the pre-vSphere 5.0 setting, delete the snapshots prior to the Storage vMotion. For more information, see [Migrating an ESX 3.x virtual machine with snapshots in powered-off or suspended state to another datastore might cause data loss and make the virtual machine unusable (1020709)](http://kb.vmware.com/kb/1020709).

vSphere 5.0 and ESXi 5.0 and later support Storage vMotion with snapshots present on a Virtual machine. For more information, see [Migrating virtual machines with snapshots (1035550)](http://kb.vmware.com/kb/1035550).

Confirm that there are no snapshots present (through command line) before increasing the size of any virtual machine virtual disk or virtual RDM. If snapshots are present, delete them prior to increasing the size of the disk. Increasing the size of a disk with snapshots present can lead to corruption of snapshots and a potential data loss. For more information, see [Increasing the Size of a Virtual Disk (1004047)](http://kb.vmware.com/kb/1004047).