# 01 Installing Domain Controller

1. Use 'sconfig' to:
    - Change the hostname
    - Change the IP address to static
    - Change the DNS server to our own IP address

2. Install Active Directory Windows Feature

```shell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools

Get-NetIPAddress
```
# 02 Joining workstation to domain

```shell
Add-Computer -Domainname xyz.com -Credential xyz\Administrator -Force -Restart
```
# 03 Execution Policies 

Information about how to allow/remove script privileges [here.](https://learn.microsoft.com/en-au/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2)

```shell
Set-ExecutionPolicy -ExecutionPolicy <PolicyName> -Scope <scope>
```
For example:
```shell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
# 04 Remote PowerShell Sessions

This applies where the client machine is not joined to the same domain as the remote machine.
You have to set the IP address of the remote machine as a Trusted Host:

```shell
Start-Service WinRM
set-item wsman:\localhost\Client\TrustedHosts -value <RemoteIPAddress>
```
You can now start a remote session through the following command:
```shell
New-PSSession -ComputerName <RemoteIPAddress> -Credential (Get-Credential)
```
To automate the process further for a particular connection, you can create a variable that executes this command with a specified input:
```shell
$credentials = (Get-Credential)
```
```shell
$remoteconnection = New-PSSession -ComputerName <RemoteIPAddress> -Credential $credentials
```
