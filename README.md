# network-profile-winserver
Change network profile on Windows Server

Network pofile change can be done using powershell

## Get InterfaceIndex 
```powershell
Get-NetConnectionProfile
```
<code>
Name             : Network
InterfaceAlias   : Ethernet0 2
InterfaceIndex   : 5
NetworkCategory  : Private
IPv4Connectivity : Internet
IPv6Connectivity : NoTraffic
</code>

## Change network profile
```powershell
Set-NetConnectionProfile -InterfaceIndex 5 -NetworkCategory DomainAuthenticated
```

### Have error ?
<code>
Set-NetConnectionProfile : Unable to set NetworkCategory to 'DomainAuthenticated'.  This NetworkCategory type will be
set automatically when authenticated to a domain network.
</code>

### Set firewall to disabled 
```powershell
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
```

### Restarting Network Location Awareness service (NLA service) helped and profile reverted back to DomainAuthenticated
```powershell
Get-Service -Name NLASvc  | Restart-Service -Force 
```

### Verify
```powershell
Get-NetConnectionProfile
```
<code>
Name             : mydomain.local
InterfaceAlias   : Ethernet0 2
InterfaceIndex   : 5
NetworkCategory  : DomainAuthenticated
IPv4Connectivity : Internet
IPv6Connectivity : NoTraffic
</code>

### Reactivate firewall
```powershell
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
```
### Its Done


