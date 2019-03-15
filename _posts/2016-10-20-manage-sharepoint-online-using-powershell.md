---
title: Manage Sharepoint Online using Powershell
comments: true
---

If you familiar enough with the sharepoint online feature and administration, we can continue to explore how to manage our sharepoint online using shell.

## Prequisite
* SharePoint Online Site (_you can get one free with O365 developer trial_)
* [SPO Management Shell](https://www.microsoft.com/en-us/download/details.aspx?id=35588)

## Open the Shell
After you have done the installation, you can open the `SPO Management Shell` by type to `sharepoint` to on your start-apps. Then right click and run as administrator.

![run as admin](http://i.imgur.com/colKtM3.jpg)

## Setup Proxy
Because our company is behind proxy, we should execute this command to use dafault proxy:

```powershell
$cred = [System.Net.CredentialCache]::DefaultCredentials
[System.Net.WebRequest]::DefaultWebProxy.Credentials = $cred
```

## Connect to SharePoint Online
After that, we can try to connect to our sharepoint, using Connect-SPOService:

```powershell
$adminUPN="<the full email address of a SharePoint Online global administrator account, example: jdoe@contosotoycompany.onmicrosoft.com>"
$orgName="<name of your Office 365 organization, example: contosotoycompany>"
$userCredential = Get-Credential -UserName $adminUPN -Message "Type the password."
Connect-SPOService -Url https://$orgName-admin.sharepoint.com -Credential $userCredential
```

below is example of mine:

```powershell
$adminUPN="putu_p@spmitrais.onmicrosoft.com"
$orgName="spmitrais"
$userCredential = Get-Credential -UserName $adminUPN -Message "why-you-soRUDe123"
Connect-SPOService -Url https://$orgName-admin.sharepoint.com -Credential $userCredential
```

## Sample Command
After you successful connect to your site, now you should try a command to manage your SharePoint. Below is sample command to create subsite on your site.

```powershell
New-SPOSite -Url https://spmitrais.sharepoint.com/sites/hellow -Owner putu_p@spmitrais.onmicros
oft.com -StorageQuota 1000 -Title "Hello it is Me!"
```

## References
You should go to [microsoft site](https://technet.microsoft.com/en-us/library/fp161388.aspx) for complete references.
