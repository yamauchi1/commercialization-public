---
author: joshbax-msft
title: Troubleshooting Device.Storage Testing
description: Troubleshooting Device.Storage Testing
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 4c504b74-0324-4b84-bc66-5a54d22803ff
---

# Troubleshooting Device.Storage Testing


To troubleshoot issues that occur with Device.Storage tests, follow these steps:

1.  Review [Troubleshooting Windows HCK Test Failures](troubleshooting-windows-hck-test-failures.md).

2.  Review one of these Windows Hardware Certification Kit (Windows HCK) topics, depending on the type of storage device or configuration:

    -   [Hard Disk Drive Testing Prerequisites](hard-disk-drive-testing-prerequisites.md)

    -   [Hardware-based Raid Systems (Fibre Channel, SAS, SCSI, Serial ATA) Testing Prerequisites](hardware-based-raid-systems--fibre-channel-sas-scsi-serial-ata--testing-prerequisites.md)

    -   [Optical Disk Drive Testing Prerequisites](optical-disk-drive-testing-prerequisites.md)

    -   [Removable Storage Testing Prerequisites](removable-storage-testing-prerequisites.md)

    -   [Secure Digital Host Controller Storage Testing Prerequisites](secure-digital-host-controller-storage-testing-prerequisites.md)

    -   [Storage Adapter or Controller Testing Overview](storage-adapter-or-controller-testing-overview.md)

3.  Review the [Windows HCK release notes](http://go.microsoft.com/fwlink/p/?LinkID=236110) for current test issues.

4.  For a test failure, look for usable information in the Windows HCK Studio test log. If you find usable information, resolve the issue and rerun the test.

5.  If you observe any failures while running the tests in the Windows HCK, look at the test logs that were generated. For example, for the Enumeration Test, the most relevant log is enumeratedrive.log.wtl. To view this log, go to the **Results** tab in the HCK studio and expand **Enumeration Test** &gt; **Test Run Date and Time** &gt; **Run Test** &gt; **Logs** &gt; **enumeratedrive.log.wtl**.

6.  To debug more, rerun the test manually from the Command Prompt (cmd), while setting the verbosity level to 4. This allows the test to log more information, such as data buffer, CDB information, and sense code. Documentation for each test contains details about the binary that is related to a particular test along with the binary location.

## Optical Storage Device


These are common issues with optical disk drive tests:

-   Some controllers that use Serial Advanced Technology Attachment (SATA) Advanced Host Controller Interface (AHCI) mode might cause the CDBs to time out. This time-out occurs most frequently in the Start Stop Unit test, where the CDB after test unit ready times out without any sense code being returned. To resolve the issue, try a different controller or configuration.

-   Some drives intermittently can't delete data from a disk. This issue might be caused by rewritable media that has been used too many times. Try to use new rewritable media.

For more information about how to troubleshoot a test, see the troubleshooting section of a specific test in [Device.Storage Tests](devicestorage-tests.md).

## Hybrid Information Device


There are special steps that can be taken to reproduce a particular test case in a test, or if necessary, conduct a manual investigation of the device.

1.  Install hybridflt. These files(.inf, .sys, .cat) are found under the same folder as hybriddrive.exe

2.  Enable Storport Tracing

3.  Run hybriddrive.exe

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Command</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Hybriddrive.exe –drive &lt;disk #&gt; -scenario &lt;scenario&gt; &lt;additional options&gt;</strong></p></td>
<td><p>Runs the test.</p></td>
</tr>
<tr class="even">
<td><p><strong>-Drive &lt;disk#&gt;</strong></p></td>
<td><p>The drive to be tested on. The behavior of boot drives or drives with a file system is not defined.</p></td>
</tr>
<tr class="odd">
<td><p><strong>-Verbosity</strong></p></td>
<td><p>The logging level for this test.</p>
<p>Default value: 1</p></td>
</tr>
<tr class="even">
<td><p><strong>-?</strong></p></td>
<td><p>Displays help.</p></td>
</tr>
<tr class="odd">
<td><p><strong>-Scenario logrw</strong></p></td>
<td><p>The scenario to run.</p></td>
</tr>
<tr class="even">
<td><p><strong>-case #</strong></p></td>
<td><p>The test case to run.</p></td>
</tr>
<tr class="odd">
<td><p>-length #(k|m|g)</p></td>
<td><p>Specifies the length of the operation.</p></td>
</tr>
<tr class="even">
<td><p>-offset #(k|m|g)</p></td>
<td><p>Specifies the offset of the operation from the start of the disk.</p></td>
</tr>
<tr class="odd">
<td><p>-tpriority #(|none)</p></td>
<td><p>Specifies the target priority of the operation. Specify <strong>none</strong> for no priority (this is different than a priority of 0).</p></td>
</tr>
<tr class="even">
<td><p>-spriority #</p></td>
<td><p>Specifies the source priority of the operation.</p></td>
</tr>
<tr class="odd">
<td><p>-thigh #</p></td>
<td><p>Specifies the high threshold.</p></td>
</tr>
<tr class="even">
<td><p>-tlow #</p></td>
<td><p>Specifies the low threshold.</p></td>
</tr>
<tr class="odd">
<td><p>-operation (r|w)</p></td>
<td><p>Specifies read or write.</p></td>
</tr>
</tbody>
</table>

 

**Test scenarios:**

-   Logverify

-   Logrw

-   Logcommand

-   Location

-   Tagperf

Manual operation:

-   Print

    -   Prints out the current state of the disk.

-   Changelba

    -   Sends down change lba by range command. Valid options for this command are length, offset, and tpriority.

-   Demote

    -   Sends down demote by size command. Valid options for this command are length, tpriority, and spriority.

-   Off

    -   Turns off the cache.

-   On

    -   Turns on the cache.

-   Evict

    -   Sends an evict command. Valid options for this command are length and offset.

-   Threshold

    -   Sets the dirty threshold. Valid options for this command are thigh and tlow.

-   Movedata

    -   Reads and writes data from the device. Valid options for this command are length, offset, tpriority, and operation. This will also set the priority for any future I/O.

-   Priority

    -   Set the priority for future reads and writes. Valid options for this command are tpriority.

**Note**  
Invalid parameters will be ignored.

Unspecified valid parameters are defaulted to a fixed value.

 

For more information about how to troubleshoot a test, see the troubleshooting section of a specific test in [Device.Storage Tests](devicestorage-tests.md).

## Related topics


[Device.Storage Testing](devicestorage-testing.md)

[Troubleshooting Windows HCK](troubleshooting-windows-hck.md)

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bp_hck\p_hck%5D:%20Troubleshooting%20Device.Storage%20Testing%20%20RELEASE:%20%284/27/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




