---
Author: andy arismendi
Publisher: 
Copyright: 
Email: 
Version: 0.1
Encoding: ascii
License: cc0
PoshCode ID: 6481
Published Date: 2016-08-19t16
Archived Date: 2016-08-21t07
---

# set-localuseraccount - 

## Description

sets properties for a given user local username.

## Comments



## Usage



## TODO



## function

`set-localuseraccount`

## Code

`#
 #
 function Set-LocalUserAccount {
 	param (
 		[parameter(Mandatory=$true)]
 		[string] $Username,
 		[string] $Description,
 		[string] $FullName,
 		[string] $ComputerName = $env:COMPUTERNAME,
 		[system.Security.SecureString] $Password,
 		[switch] $PasswordChangeAtNextLogon,
 		[switch] $CannotChangePassword,
 		[switch] $PasswordNeverExpires,
 		[switch] $Enable,
 		[switch] $Disable,
 		[switch] $UnLock,
 		[switch] $ResetAllFlags
 	)
 	
 	try {
 		if ($Enable -and $Disable) {
 			Write-Warning "Please use only -Enable or -Disable."; return
 		}
 		
 		if ($Password) {
 			$pass = [Runtime.InteropServices.marshal]::PtrToStringAuto([Runtime.InteropServices.marshal]::SecureStringToBSTR($Password))
 		}
 		
 		$AccountOptions = @{
 			ACCOUNTDISABLE = 2; LOCKOUT = 16; PASSWD_CANT_CHANGE = 64; NORMAL_ACCOUNT = 512; DONT_EXPIRE_PASSWD = 65536
 		}
 		
 		$user = [ADSI] "WinNT://$ComputerName/$Username"
 		
 		if ($Description) {$user.Description = $Description}
 		
 		if ($FullName) {$user.FullName = $FullName}
 		
 		if ($pass) {
 			$user.psbase.invoke("SetPassword", $pass)
 			$user.psbase.CommitChanges()
 		}
 		
 		if ($ResetAllFlags) {
 			$user.UserFlags = $user.UserFlags.Value -band $AccountOptions.NORMAL_ACCOUNT
 		} else {
 			if ($PasswordChangeAtNextLogon) {
 				
 				$user.UserFlags = $AccountOptions.PASSWD_CANT_CHANGE -band $AccountOptions.DONT_EXPIRE_PASSWD
 				$user.PasswordExpired = 1
 
 			} else {
 				if ($CannotChangePassword) {
 					$user.PasswordExpired = 0
 					$user.UserFlags = $user.UserFlags.Value -bor $AccountOptions.PASSWD_CANT_CHANGE
 				} 
 				if ($PasswordNeverExpires) {$user.UserFlags = $user.UserFlags.Value -bor $AccountOptions.DONT_EXPIRE_PASSWD}	
 			}
 			
 			if ($Enable) {$user.InvokeSet("AccountDisabled", "False")}
 			
 			if ($Disable) {$user.InvokeSet("AccountDisabled", "True")}
 			
 			if ($UnLock) {$user.IsAccountLocked = $false}
 		}
 		$user.SetInfo()
 	} catch {
 		throw 'Failed to set local user account properties. The error was: "{0}".' -f $_
 	}	
 }
`

