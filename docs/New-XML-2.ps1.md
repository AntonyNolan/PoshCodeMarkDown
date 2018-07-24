---
Author: joel bennett
Publisher: 
Copyright: 
Email: 
Version: 3.0
Encoding: ascii
License: cc0
PoshCode ID: 1244
Published Date: 2010-07-29t15
Archived Date: 2017-01-01t08
---

# new-xml 2 - 

## Description

an update to my mini-dsl for generating xml documents.

## Comments



## Usage



## TODO



## function

`new-xml`

## Code

`#
 #
 ####################################################################################################
 $xlr8r = [type]::gettype("System.Management.Automation.TypeAccelerators")
 $xlinq = [Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089")
 $xlinq.GetTypes() | ? { $_.IsPublic -and !$_.IsSerializable -and $_.Name -ne "Extensions" -and !$xlr8r::Get[$_.Name] } | % {
   $xlr8r::Add( $_.Name, $_.FullName )
 }
 
 function New-Xml {
 Param(
    [Parameter(Mandatory = $true, Position = 0)]
    [System.Xml.Linq.XName]$root
 ,
    [Parameter(Mandatory = $false)]
    [string]$Version = "1.0"
 ,
    [Parameter(Mandatory = $false)]
    [string]$Encoding = "UTF-8"
 ,
    [Parameter(Mandatory = $false)]
    [string]$Standalone = "yes"
 ,
    [Parameter(Position=99, Mandatory = $false, ValueFromRemainingArguments=$true)]
    [PSObject[]]$args
 )
 BEGIN {
    if(![string]::IsNullOrEmpty( $root.NamespaceName )) {
       Function New-XmlDefaultElement {
          Param([System.Xml.Linq.XName]$tag)
          if([string]::IsNullOrEmpty( $tag.NamespaceName )) {
             $tag = $($root.Namespace) + $tag
          }
          New-XmlElement $tag @args
       }
       Set-Alias xe New-XmlDefaultElement
    }
 }
 PROCESS {
    New-Object XDocument (New-Object XDeclaration $Version, $Encoding, $standalone),(
       New-Object XElement $(
          $root
          while($args) {
             $attrib, $value, $args = $args
             if($attrib -is [ScriptBlock]) {
                &$attrib
             } elseif ( $value -is [ScriptBlock] -and "-Content".StartsWith($attrib)) {
                &$value
             } elseif ( $value -is [XNamespace]) {
                New-XmlAttribute ([XNamespace]::Xmlns + $attrib.TrimStart("-")) $value
             } else {
                New-XmlAttribute $attrib.TrimStart("-") $value
             }
          }
       ))
 }
 END {
    Set-Alias xe New-XmlElement
 }
 }
 function New-XmlAttribute {
 Param($name,$value)
    New-Object XAttribute $name,$value
 }
 Set-Alias xa New-XmlAttribute
 
 
 function New-XmlElement {
   Param([System.Xml.Linq.XName]$tag)
   Write-Verbose $($args | %{ $_ | Out-String } | Out-String)
   New-Object XElement $(
      $tag
      while($args) {
         $attrib, $value, $args = $args
         if($attrib -is [ScriptBlock]) {
            &$attrib
         } elseif ( $value -is [ScriptBlock] -and "-Content".StartsWith($attrib)) {
            &$value
         } elseif ( $value -is [XNamespace]) {
             New-Object XAttribute ([XNamespace]::Xmlns + $attrib.TrimStart("-")),$value
         } else {
            New-Object XAttribute $attrib.TrimStart("-"), $value
         }
      }
    )
 }
 Set-Alias xe New-XmlElement
 
 
 
 
 ####################################################################################################
 #
 #
 #
 
 
 ####################################################################################################
 ####################################################################################################
 #
 #
 #
 #
`

