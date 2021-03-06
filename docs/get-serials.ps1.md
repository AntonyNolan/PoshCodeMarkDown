---
Author: jeff patton
Publisher: 
Copyright: 
Email: 
Version: 0.1
Encoding: ascii
License: cc0
PoshCode ID: 2873
Published Date: 2012-07-28t14
Archived Date: 2017-05-22t04
---

# get-serials - 

## Description

return a list of computers with their serial numbers. for dell computers the win32_bios.serialnumber property is the service tag of the computer. this identifies the computer on the dell support site, and with it you can get the proper drivers/manuals and warranty information.

## Comments



## Usage



## TODO



## script

``

## Code

`#
 #
 <#
     .SYNOPSIS
         Get the BIOS serial numbers from computers in the domain.
     .DESCRIPTION
         Return a list of computers with their serial numbers. For Dell computers the Win32_BIOS.SerialNumber property
         is the service tag of the computer. This identifies the computer on the Dell support site, and with it you can
         get the proper drivers/manuals and warranty information.
     .PARAMETER ADSPath
         The location within Active Directory to find computers.
     .EXAMPLE
         .\get-serials.ps1 -ADSPath "LDAP://OU=workstations,DC=company,DC=com"
 
         ComputerName                                                ServiceTag
         ------------                                                ----------
         Desktop-pc01                                                a1b2c3d
         Desktop-pc02                                                b2c3d4e
         Desktop-pc03                                                The RPC server is unavailable. (Exception from H ...
         Desktop-pc04                                                f4g5h6i
 
         Description
         -----------
         This example shows the output of the command.
     .EXAMPLE
         .\get-serials.ps1 -ADSPath "LDAP://OU=workstations,DC=company,DC=com" `
         | Export-Csv .\sample.csv -NoTypeInformation
         
         Description
         -----------
         This example shows piping the output to a csv file.
     .NOTES
     .LINK
         http://scripts.patton-tech.com/wiki/PowerShell/Production/GetSerials
 #>
 
 Param
     (
         [Parameter(Mandatory=$true)]
         $ADSPath
     )
 Begin
     {    
         $ScriptName = $MyInvocation.MyCommand.ToString()
         $LogName = "Application"
         $ScriptPath = $MyInvocation.MyCommand.Path
         $Username = $env:USERDOMAIN + "\" + $env:USERNAME
 
         New-EventLog -Source $ScriptName -LogName $LogName -ErrorAction SilentlyContinue
         	
         $Message = "Script: " + $ScriptPath + "`nScript User: " + $Username + "`nStarted: " + (Get-Date).toString()
         Write-EventLog -LogName $LogName -Source $ScriptName -EventID "100" -EntryType "Information" -Message $Message 
         . .\includes\ActiveDirectoryManagement.ps1
     }
 Process
     {
     	$Workstations = Get-ADObjects -ADSPath $ADSPath
         
         $Jobs = @()
     	foreach ($Workstation in $Workstations)
     		{
                 [string]$ThisWorkstation = $Workstation.Properties.name
                 $ThisJob = New-Object PSobject
     			if ($Workstation -eq $null){}
     			else
     				{
                         Try
                             {
                                 $ErrorActionPreference = "Stop"
                                 $Serial = Get-WmiObject -query "select SerialNumber from win32_bios" `
                                         -computername $ThisWorkstation
                                 $Return = $serial.serialnumber
                             }
                         Catch
                             {
                                 [string]$ThisError = $Error[0].Exception
                                 $ThisError = $ThisError.Substring($ThisError.IndexOf(":"))
                                 $ThisError = $ThisError.Substring(1,$ThisError.IndexOf("`r"))
                                 $return = $ThisError.Trim()
                             }
                         
                         Add-Member -InputObject $ThisJob -MemberType NoteProperty -Name "ComputerName" `
                             -Value $ThisWorkstation
                         Add-Member -InputObject $ThisJob -MemberType NoteProperty -Name "ServiceTag" -Value $Return
     				}
                 $Jobs += $ThisJob
                 $ThisJob
     		}
 
         $Message = [system.string]::Join("`n",($Jobs))
         Write-EventLog -LogName $LogName -Source $ScriptName -EventId "101" -EntryType "Information" -Message $Message
     }
 End
     {        
     	$Message = "Script: " + $ScriptPath + "`nScript User: " + $Username + "`nFinished: " + (Get-Date).toString()
     	Write-EventLog -LogName $LogName -Source $ScriptName -EventID "100" -EntryType "Information" -Message $Message
     }
`

