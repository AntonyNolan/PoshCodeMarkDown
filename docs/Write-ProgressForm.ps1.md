---
Author: denis st-pierre
Publisher: 
Copyright: 
Email: 
Version: 1.0
Encoding: ascii
License: cc0
PoshCode ID: 4019
Published Date: 2014-03-15t17
Archived Date: 2014-04-17t20
---

# write-progressform - 

## Description

gui replacement for powershell’s write-progress command

## Comments



## Usage



## TODO



## function

`write-progressform`

## Code

`#
 #
 function Write-ProgressForm {
 	<#
 	.SYNOPSIS
 		Write-ProgressForm V1.0
 		GUI Replacement for PowerShell's Write-Progress command
 
 	.DESCRIPTION
 		GUI Replacement for PowerShell's Write-Progress command
 		Uses same named parameters for drop-in replacement
 		CAVEATS: You can't close the Form by clicking on it, must call Write-ProgressForm -Completed
 				 Therefore decided to hide Close button (why tease people?)
 
 	.INPUTS
 		Same as Write-Progress command 
 		BONUS:
 		-ICOpath <path to your ICO>
 		-FormTitle <Title of the Write-progress form> 
 		(Only needs to be mentioned when 1st called then it sticks around)
 		
 	.OUTPUTS
 		Nothing but a GUI
 
 	.NOTES
 		Uses Global variables extensively
 		License: GPL v3.0 or greater / BSD (Latest version)
 		Form Code Generated By: SAPIEN Technologies PrimalForms (Community Edition) v1.0.10.0
 		Generated On: 30/01/2013 12:38 PM
 		Generated By: Denis St-Pierre, Ottawa, Canada
 	#>
 	
 		[String]$Activity="Activity",
 		[String]$CurrentOperation="CurrentOperation",
 		[String]$status="Status",
 		[Int]$PercentComplete=0,
 		[switch]$Completed=$false,
 		[int]$FontSize = 10,
 		[String]$FormTitle=""
 		
 	)
 	
 	
 	If ( Get-Variable -Name WriteProgressForm -Scope Global -ErrorAction SilentlyContinue ) {
 			$progressBar1.Value=100
 			$StatusLabel.Text = "Complete"
 			Start-Sleep -Seconds 1
 			$WriteProgressForm.close()
 			Return
 			$ActivityLabel.Text = "$Activity"
 			$CurrentOperationLabel.Text = "$CurrentOperation"
 			$StatusLabel.Text = "$status"
 			Return
 		}
 	}
 
 	[reflection.assembly]::loadwithpartialname("System.Windows.Forms") | Out-Null
 	[reflection.assembly]::loadwithpartialname("System.Drawing") | Out-Null
 
 	$global:WriteProgressForm = New-Object System.Windows.Forms.Form
 	$global:StatusLabel = New-Object System.Windows.Forms.Label
 	$CloseButton = New-Object System.Windows.Forms.Button
 	$global:progressBar1 = New-Object System.Windows.Forms.ProgressBar
 	$global:CurrentOperationLabel = New-Object System.Windows.Forms.Label
 	$global:ActivityLabel = New-Object System.Windows.Forms.Label
 	$InitialFormWindowState = New-Object System.Windows.Forms.FormWindowState
 
 	#----------------------------------------------
 	#----------------------------------------------
 
 	$CloseButton_OnClick= 
 	{
 
 	}
 
 	$OnLoadForm_StateCorrection=
 		$WriteProgressForm.WindowState = $InitialFormWindowState
 	}
 
 	#----------------------------------------------
 	$WriteProgressForm.AccessibleDescription = "WriteProgressFormDesc"
 	$WriteProgressForm.AccessibleName = "WriteProgressForm"
 	$WriteProgressForm.AccessibleRole = 48
 	$WriteProgressForm.AutoSizeMode = 0
 	$System_Drawing_Size = New-Object System.Drawing.Size
 	$WriteProgressForm.DataBindings.DefaultDataSourceUpdateMode = 0
 	If ( ($ICOpath -ne "") -and (Test-Path "$ICOpath") ) {
 			$WriteProgressForm.Icon = [System.Drawing.Icon]::ExtractAssociatedIcon("$ICOpath")
 	}
 	$WriteProgressForm.Name = "WriteProgressForm"
 	$WriteProgressForm.Text = "$FormTitle"
 
 	$StatusLabel.DataBindings.DefaultDataSourceUpdateMode = 0
 	$System_Drawing_Point = New-Object System.Drawing.Point
 	$System_Drawing_Point.X = 15
 	$System_Drawing_Point.Y = 33
 	$StatusLabel.Location = $System_Drawing_Point
 	$System_Drawing_Size = New-Object System.Drawing.Size
 	$System_Drawing_Size.Height = 20
 	$System_Drawing_Size.Width = 475
 	$StatusLabel.MinimumSize = $System_Drawing_Size
 	$StatusLabel.AutoSize = $UselessAutoSize
 	$StatusLabel.Name = "StatusLabel"
 	$System_Windows_Forms_Padding = New-Object System.Windows.Forms.Padding
 	$System_Windows_Forms_Padding.All = 1
 	$System_Windows_Forms_Padding.Bottom = 1
 	$System_Windows_Forms_Padding.Left = 1
 	$System_Windows_Forms_Padding.Right = 1
 	$System_Windows_Forms_Padding.Top = 1
 	$StatusLabel.Padding = $System_Windows_Forms_Padding
 	$StatusLabel.TabIndex = 4
 	$StatusLabel.Text = "$status"
 	
 	$WriteProgressForm.Controls.Add($StatusLabel)
 	
 	$CloseButton.DataBindings.DefaultDataSourceUpdateMode = 0
 	$System_Drawing_Point = New-Object System.Drawing.Point
 	$System_Drawing_Point.X = 400
 	$System_Drawing_Point.Y = 134
 	$CloseButton.Location = $System_Drawing_Point
 	$CloseButton.Name = "CloseButton"
 	$System_Drawing_Size = New-Object System.Drawing.Size
 	$System_Drawing_Size.Height = 25
 	$System_Drawing_Size.Width = 90
 	$CloseButton.Size = $System_Drawing_Size
 	$CloseButton.TabIndex = 3
 	$CloseButton.Text = "Close"
 	$CloseButton.UseVisualStyleBackColor = $True
 	$CloseButton.add_Click($CloseButton_OnClick)
 	
 	If ($HideUselessCloseButton) {
 	} else {
 		$WriteProgressForm.Controls.Add($CloseButton)
 	}
 	
 	$progressBar1.DataBindings.DefaultDataSourceUpdateMode = 0
 	$System_Drawing_Point = New-Object System.Drawing.Point
 	$System_Drawing_Point.X = 10
 	$System_Drawing_Point.Y = 57
 	$progressBar1.Location = $System_Drawing_Point
 	$System_Windows_Forms_Padding = New-Object System.Windows.Forms.Padding
 	$System_Windows_Forms_Padding.All = 5
 	$System_Windows_Forms_Padding.Bottom = 5
 	$System_Windows_Forms_Padding.Left = 5
 	$System_Windows_Forms_Padding.Right = 5
 	$System_Windows_Forms_Padding.Top = 5
 	$progressBar1.Margin = $System_Windows_Forms_Padding
 	$System_Drawing_Size = New-Object System.Drawing.Size
 	$System_Drawing_Size.Height = 30
 	$System_Drawing_Size.Width = 480
 	$progressBar1.MinimumSize = $System_Drawing_Size
 	$progressBar1.Name = "progressBar1"
 	$progressBar1.Step = 1
 	$progressBar1.Style = 1
 	$progressBar1.TabIndex = 2
 	
 	$WriteProgressForm.Controls.Add($progressBar1)
 	
 	$CurrentOperationLabel.AccessibleDescription = "CurrentOperationDesc"
 	$CurrentOperationLabel.AccessibleName = "CurrentOperation"
 	$CurrentOperationLabel.AccessibleRole = 0
 	$CurrentOperationLabel.CausesValidation = $False
 	$CurrentOperationLabel.DataBindings.DefaultDataSourceUpdateMode = 0
 	$System_Drawing_Point = New-Object System.Drawing.Point
 	$System_Drawing_Point.X = 15
 	$System_Drawing_Point.Y = 100
 	$CurrentOperationLabel.Location = $System_Drawing_Point
 	$System_Windows_Forms_Padding = New-Object System.Windows.Forms.Padding
 	$System_Windows_Forms_Padding.All = 1
 	$System_Windows_Forms_Padding.Bottom = 1
 	$System_Windows_Forms_Padding.Left = 1
 	$System_Windows_Forms_Padding.Right = 1
 	$System_Windows_Forms_Padding.Top = 1
 	$CurrentOperationLabel.Margin = $System_Windows_Forms_Padding
 	$System_Drawing_Size = New-Object System.Drawing.Size
 	$System_Drawing_Size.Height = 20
 	$System_Drawing_Size.Width = 475
 	$CurrentOperationLabel.MinimumSize = $System_Drawing_Size
 	$CurrentOperationLabel.AutoSize = $UselessAutoSize
 	$CurrentOperationLabel.Name = "CurrentOperationLabel"
 	$System_Windows_Forms_Padding = New-Object System.Windows.Forms.Padding
 	$System_Windows_Forms_Padding.All = 1
 	$System_Windows_Forms_Padding.Bottom = 1
 	$System_Windows_Forms_Padding.Left = 1
 	$System_Windows_Forms_Padding.Right = 1
 	$System_Windows_Forms_Padding.Top = 1
 	$CurrentOperationLabel.Padding = $System_Windows_Forms_Padding
 	$CurrentOperationLabel.TabIndex = 1
 	$CurrentOperationLabel.Text = "$CurrentOperation"
 	
 	$WriteProgressForm.Controls.Add($CurrentOperationLabel)
 	
 	$ActivityLabel.DataBindings.DefaultDataSourceUpdateMode = 0
 	$System_Drawing_Point = New-Object System.Drawing.Point
 	$System_Drawing_Point.X = 10
 	$System_Drawing_Point.Y = 10
 	$ActivityLabel.Location = $System_Drawing_Point
 	$System_Windows_Forms_Padding = New-Object System.Windows.Forms.Padding
 	$System_Windows_Forms_Padding.All = 1
 	$System_Windows_Forms_Padding.Bottom = 1
 	$System_Windows_Forms_Padding.Left = 1
 	$System_Windows_Forms_Padding.Right = 1
 	$System_Windows_Forms_Padding.Top = 1
 	$ActivityLabel.Margin = $System_Windows_Forms_Padding
 	$System_Drawing_Size = New-Object System.Drawing.Size
 	$System_Drawing_Size.Height = 20
 	$System_Drawing_Size.Width = 480
 	$ActivityLabel.MinimumSize = $System_Drawing_Size
 	$ActivityLabel.Name = "ActivityLabel"
 	$System_Windows_Forms_Padding = New-Object System.Windows.Forms.Padding
 	$System_Windows_Forms_Padding.All = 1
 	$System_Windows_Forms_Padding.Bottom = 1
 	$System_Windows_Forms_Padding.Left = 1
 	$System_Windows_Forms_Padding.Right = 1
 	$System_Windows_Forms_Padding.Top = 1
 	$ActivityLabel.Padding = $System_Windows_Forms_Padding
 	$ActivityLabel.AutoSize = $UselessAutoSize
 	$ActivityLabel.TabIndex = 0
 	$ActivityLabel.Font = New-Object System.Drawing.Font("Microsoft Sans Serif",$FontSize,1,3,1)
 	$ActivityLabel.Text = "$Activity"
 
 	$WriteProgressForm.Controls.Add($ActivityLabel)
 
 
 	$InitialFormWindowState = $WriteProgressForm.WindowState
 	$WriteProgressForm.add_Load($OnLoadForm_StateCorrection)
 	
 
 
 try {
 	$WriteProgressForm.close()
 	Remove-Variable StatusLabel | Out-Null
 	Remove-Variable CurrentOperationLabel | Out-Null
 	Remove-Variable progressBar1 | Out-Null
 	Remove-Variable ActivityLabel | Out-Null
 	Remove-Variable WriteProgressForm
 } catch {
 }
 
 $ScopeNum=0
 $Scope=0
 $TotalNumScopes=100
 	
 for ($PCcomplete=1; $PCcomplete -lt 100; $PCcomplete++) {
 	$ScopeName="ScopeName"
 	$CurrentOperation="*CurOp*Getting Leases from scope $ScopeName"
 	$Status="*Status*$PCcomplete% complete. ($Scope of $TotalNumScopes)"
 	Write-ProgressForm -Activity "*Act*Building list from DHCP server ... Building list from DHCP server... Building list from DHCP server ..." -status $status -PercentComplete $PCcomplete -CurrentOperation "$CurrentOperation"
 	Start-sleep -Milliseconds 050
 	
 }
 $PCcomplete=100
 $CurrentOperation="complete"
 Write-ProgressForm -Activity "Building list from DHCP server ..." -PercentComplete $PCcomplete -CurrentOperation "$CurrentOperation" -Status "$PCcomplete% complete."
 Start-sleep -seconds 1
 Write-ProgressForm -Activity "Complete" -Completed -Status "Complete"
`

