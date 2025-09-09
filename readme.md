<p align="center">
    <a href="https://scloud.work" alt="Florian Salzmann | scloud"></a>
            <img src="https://scloud.work/wp-content/uploads/EntraGroup.Toolbox-Icon.png" width="75" height="75" /></a>
</p>
<p align="center">
    <a href="https://www.linkedin.com/in/fsalzmann/">
        <img alt="Made by" src="https://img.shields.io/static/v1?label=made%20by&message=Florian%20Salzmann&color=04D361">
    </a>
    <a href="https://x.com/FlorianSLZ" alt="X / Twitter">
    	<img src="https://img.shields.io/twitter/follow/FlorianSLZ.svg?style=social"/>
    </a>
</p>
<p align="center">
    <a href="https://www.powershellgallery.com/packages/EntraGroup.Toolbox/" alt="PowerShell Gallery Version">
        <img src="https://img.shields.io/powershellgallery/v/EntraGroup.Toolbox.svg" />
    </a>
    <a href="https://www.powershellgallery.com/packages/EntraGroup.Toolbox/" alt="PS Gallery Downloads">
        <img src="https://img.shields.io/powershellgallery/dt/EntraGroup.Toolbox.svg" />
    </a>
</p>
<p align="center">
    <a href="https://raw.githubusercontent.com/FlorianSLZ/EntraGroup.Toolbox/master/LICENSE" alt="GitHub License">
        <img src="https://img.shields.io/github/license/FlorianSLZ/EntraGroup.Toolbox.svg" />
    </a>
    <a href="https://github.com/FlorianSLZ/EntraGroup.Toolbox/graphs/contributors" alt="GitHub Contributors">
        <img src="https://img.shields.io/github/contributors/FlorianSLZ/EntraGroup.Toolbox.svg"/>
    </a>
</p>

<p align="center">
    <a href='https://ko-fi.com/elflorian' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi1.png?v=3' border='0' alt='Buy Me a Glass of wine' /></a>
</p>

# EntraGroup.Toolbox



## Installing the module from PSGallery

The EntraGroup.Toolbox module is published to the [PowerShell Gallery](https://www.powershellgallery.com/packages/EntraGroup.Toolbox). 
Install it on your system by running the following in an elevated PowerShell console:
```PowerShell
Install-Module -Name EntraGroup.Toolbox
```

## Import the module for testing

As an alternative to installing, you chan download this Repository and import it in a PowerShell Session. 
*The path may be different in your case*
```PowerShell
Import-Module -Name "C:\GitHub\EntraGroup.Toolbox\EntraGroup.Toolbox" -Verbose -Force
```

## Module dependencies

EntraGroup.Toolbox module requires the following modules, which will be automatically installed as dependencies:
- Microsoft.Graph.Authentication
- Microsoft.Graph.Identity.DirectoryManagement

# Functions / Examples

Here are all functions and some examples to start with:

- Set-GroupByPrimaryUser


## Authentication
Before using the "Get-EntraGroup.Toolbox-Validation" function within this module, ensure you are authenticated. 

### User Authentication
With this command, you'll be connected to the Graph API and be able to use all commands
```PowerShell
Connect-MgGraph -Scopes "Group.ReadWrite.All", "User.Read.All", "Device.Read.All", "GroupMember.ReadWrite.All"
```

