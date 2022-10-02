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
You can enter the session through your terminal with the following command:
```shell
Enter-PSSession $remoteconnection
```
While running an active session, you are able to execute commands remotely from within your own terminal. For example, to copy files across to the remote machine:
```shell
 cp .\AD_Generate.ps1 -ToSession $remoteconnection C:\Windows\Tasks
```
```shell
 cp .\AD_UserList_Example.json -ToSession $remoteconnection C:\Windows\Tasks
```
# 05 Removing Userlists and Password Policies

If you would like to remove the users you have automated into the domain, execute the AD_Generate.ps1 script on the domain controller with the desired userlist file and add an <-undo> argument:

```shell
.\AD_Generate.ps1 .\AD_UserList_Example.json -Undo
```
You can check the users which exist in the domain with the following command:
```shell
net user /domain
```
