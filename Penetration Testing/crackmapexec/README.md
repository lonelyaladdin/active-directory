# crackmapexec

Github repository for CrackMapExec [here.](https://github.com/Porchetta-Industries/CrackMapExec)

# 1. Target Formats

Every protocol supports targets by CIDR notation(s), IP address(s), IP range(s), hostname(s), a file containing a list of targets or combination of all of the latter:

```shell 
crackmapexec <protocol> ms.evilcorp.org
crackmapexec <protocol> 192.168.1.0 192.168.0.2
crackmapexec <protocol> 192.168.1.0/24
crackmapexec <protocol> 192.168.1.0-28 10.0.0.1-67
crackmapexec <protocol> ~/targets.txt
```

# 2. Using Credentials

Every protocol supports using credentials in one form or another. Generally speaking, to use credentials, you can run the following commands:

```shell
crackmapexec <protocol> <target(s)> -u username -p password
```
When using usernames or passwords that contain special symbols, wrap them in single quotes to make your shell interpret them as a string.
For example:
```shell
crackmapexec <protocol> <target(s)> -u username -p 'Admin!123@'
```
# 3. Brute Forcing & Password Spraying

All protocols support brute-forcing and password spraying. By specifying a file or multiple values CME will automatically brute-force logins for all targets using the specified protocol. Examples:

```shell
crackmapexec <protocol> <target(s)> -u username1 -p password1 password2
```
```shell
crackmapexec <protocol> <target(s)> -u username1 username2 -p password1
```
```shell
crackmapexec <protocol> <target(s)> -u ~/file_containing_usernames -p ~/file_containing_passwords
```
```shell
crackmapexec <protocol> <target(s)> -u ~/file_containing_usernames -H ~/file_containing_ntlm_hashes
```