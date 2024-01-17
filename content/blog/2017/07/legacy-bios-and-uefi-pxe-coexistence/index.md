---
title: "Legacy BIOS and UEFI PXE coexistence"
date: "2017-07-17"
categories: 
  - "system-administration"
  - "tech"
tags: 
  - "bios"
  - "dhcp"
  - "powershell"
  - "pxe"
  - "sccm"
  - "sysadmin"
  - "technology"
  - "uefi"
  - "windows-server-2012-r2"
coverImage: "bios_uefi_cover.jpg"
---

More and more enterprises are moving towards the modern UEFI devices in their fleet, whether that be desktops, laptops or convertibles. With this migration comes a change in how they boot, including off the network utilising the PXE system to grab a operating system image of some kind (like Microsoft MDT which then splats a full blown image on to the devices).

While this change is most welcome, as UEFI has more capabilities, more secure and incredibly quick which comparing to the older legacy BIOS boot management, it does mean (possibly) supporting both systems for a period of time.

You may have stood up a new, clean system like SCCM to build these fancy, shiny devices and moved all your options in DHCP to point to said system, but what happens to the older system. DHCP scopes can only have one of each option defined in Windows Server instances. Rather than standing up a separate network (or VLAN), a separate DHCP server or a combination of the two; there is a way to support both in a more efficient manner.

## Enter DHCP vendor classes and policies

Vendor classes in Windows Server DHCP are used to identify DHCP clients beyond their MAC address and the corresponding IP address they are allocated. This is done in the makeup of the packet sent via DHCP and PXE protocols from client to server as explained in [RFC 4578 of the IETF](https://tools.ietf.org/html/rfc4578). The crux of this specification is the following types of clients:

| Type | Architecture Name |
|---|---|
| 0 | Intel x86PC |
| 1 | NEC/PC98 |
| 2 | EFI Itanium |
| 3 | DEC Alpha |
| 4 | Arc x86 |
| 5 | Intel Lean Client |
| 6 | EFI IA32 |
| 7 | EFI BC |
| 8 | EFI Xscale |
| 9 | EFI x86-64 |

Notice both type 0, which is the legacy BIOS style boot system and types 2, 6, 7, 8, 9 which are all UEFI style boot systems (although most consumer/workstation devices will be type 9).

In our DHCP system, we want these type 0 or legacy BIOS systems to go to the previous PXE environment. We achieve this with the use of DHCP policies.

DHCP policies in Windows Server DHCP are rules that use conditions to apply to a subset of clients (in our case, we use the Vendor class we will define later on) a set of DHCP options such as option 66, and 67 which define PXE environment locations.

With both vendor classes and policies in our arsenal, lets combine the two with DHCP Powershell commands to allow both systems to coexist.

## Add vendor class and policy to chosen scopes

The below script will prompt the user for which DHCP scopes (in _10.x.x.1,192.x.x.1_ format) will receive the new policy and add the necessary vendor class at the DHCP server level. Combined, they will direct BIOS based clients to your older PXE environment instead of attempting to connect to your new fangdangled one.

{{< gist dxpetti d11f5e812ff3a31a2cbedecac796301e >}}

The only thing you will need to do is swap out **"PXESERVER.local"** and **"SMSBoot\x86\wdsnbp.com"** for the FQDN of your legacy PXE environment server and the path to the boot file respectively.

Once these values are in and the script is in a .PS1 powershell script file, run it and you will be asked to enter the scopes requiring the coexistence of the legacy and new PXE environments.

But what about if you no longer need the coexistence as you slowly migrate the endpoints to UEFI hardware?

## Remove vendor class and policy to chosen scopes

The below script can be ran, whereby, once again you will be prompted for the scope details for removal of the policy.

{{< gist dxpetti 6f62e7ed9835892fef510f954de69b96 >}}

No edits required on the above.

The script is built with the assumation that the legacy environment will be supported for some time and thus does not remove the vendor class from the DHCP servers. However, what if you want to remove the defined vendor class from the environment too (assuming there is no more legacy endpoints to support)?

The below one liner will clear it out:

```powershell
$dhcpServers = Get-DhcpServerInDC | foreach ($dhcpServer in $dhcpServers) { Remove-DHCPServerv4Class -ComputerName $dhcpServer -Name "PXEClient(Legacy BIOS)" -Type Vendor }
```

Gone, no more legacy vendor class.

It seems, despite the apparent limitations of DHCP in Windows Server, two worlds can happily live together.
