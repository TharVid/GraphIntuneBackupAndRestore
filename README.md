

# Intune Backup & Restore

This PowerShell Module queries Microsoft Graph and allows for cross-tenant Backup & Restore actions of your Intune Configuration.

Intune Configuration is backed up as (json) files in a given directory.

## Removing Old Version

If you have an older version of the module installed from the PowerShell Gallery, you should uninstall it first:

```powershell
Uninstall-Module -Name IntuneBackupAndRestore


## Importing the New Version

To use the new version of the module from your local repository, follow these steps:

1. **Clone the repository:**

   ```bash
   git clone https://github.com/TharVid/GraphIntuneBackupAndRestore
   ```

2. **Navigate to the module directory:**

   ```powershell
   cd GraphIntuneBackupAndRestore\IntuneBackupAndRestore
   ```

3. **Import the module:**

   ```powershell
   Import-Module .\IntuneBackupAndRestore.psm1
   ```

4. **Connect to Microsoft Graph:**

   ```powershell
   Connect-MgGraph
   ```

5. **Verify the import:**

   ```powershell
   Get-Module -Name IntuneBackupAndRestore
   ```

## Prerequisites

- Requires [Microsoft.Graph](https://github.com/microsoftgraph/msgraph-sdk-powershell) PowerShell Module (`Install-Module -Name Microsoft.Graph`, `Install-Module Microsoft.Graph.Beta -AllowClobber`)
- Make sure to import the IntuneBackupAndRestore PowerShell module before using it with the `Import-Module IntuneBackupAndRestore` cmdlet.

## Features

### Backup actions

- Administrative Templates (Device Configurations)
- Administrative Template Assignments
- App Protection Policies
- App Protection Policy Assignments
- Client Apps
- Client App Assignments
- Device Compliance Policies
- Device Compliance Policy Assignments
- Device Configurations
- Device Configuration Assignments
- Device Management Scripts (Device Configuration -> PowerShell Scripts)
- Device Management Script Assignments
- Proactive Remediations
- Proactive Remediation Assignments
- Settings Catalog Policies
- Settings Catalog Policy Assignments
- Software Update Rings
- Software Update Ring Assignments
- Endpoint Security Configurations
  - Security Baselines
    - Windows 10 Security Baselines
    - Microsoft Defender ATP Baselines
    - Microsoft Edge Baseline
  - Antivirus
  - Disk encryption
  - Firewall
  - Endpoint detection and response
  - Attack surface reduction
  - Account protection
  - Device compliance

### Restore actions

- Administrative Templates (Device Configurations)
- Administrative Template Assignments
- App Protection Policies
- App Protection Policy Assignments
- Client App Assignments
- Device Compliance Policies
- Device Compliance Policy Assignments
- Device Configurations
- Device Configuration Assignments
- Device Management Scripts (Device Configuration -> PowerShell Scripts)
- Device Management Script Assignments
- Settings Catalog Policies
- Settings Catalog Policy Assignments
- Software Update Rings
- Software Update Ring Assignments
- Endpoint Security Configurations
  - Security Baselines
    - Windows 10 Security Baselines
    - Microsoft Defender ATP Baselines
    - Microsoft Edge Baseline
  - Antivirus
  - Disk encryption
  - Firewall
  - Endpoint detection and response
  - Attack surface reduction
  - Account protection
  - Device compliance

> Please note that some Client App settings can be backed up, for instance the retrieval of Win32 (un)install cmdlets, requirements, etcetera. The Client App itself is not backed up and this module does not support restoring Client Apps at this time.

## Examples

### Example 01 - Full Intune Backup

```powershell
Start-IntuneBackup -Path C:\temp\IntuneBackup
```

### Example 02 - Full Intune Restore

```powershell
Start-IntuneRestoreConfig -Path C:\temp\IntuneBackup
Start-IntuneRestoreAssignments -Path C:\temp\IntuneBackup
```

### Example 03 - Restore Intune Assignments

If configurations have been restored:

```powershell
Start-IntuneRestoreAssignments -Path C:\temp\IntuneBackup
```

If reassigning assignments to existing (non-restored) configurations. In this case, the assignments match the configuration id to restore to.  
This allows for restoring if display names have changed.

```powershell
Start-IntuneRestoreAssignments -Path C:\temp\IntuneBackup -RestoreById $true
```

### Example 04 - Restore only Intune Compliance Policies

```powershell
Invoke-IntuneRestoreDeviceCompliancePolicy -Path C:\temp\IntuneBackup
```

```powershell
Invoke-IntuneRestoreDeviceCompliancePolicyAssignment -Path C:\temp\IntuneBackup
```

### Example 05 - Restore Only Intune Device Configurations

```powershell
Invoke-IntuneRestoreDeviceConfiguration -Path C:\temp\IntuneBackup
```

```powershell
Invoke-IntuneRestoreDeviceConfigurationAssignment -Path C:\temp\IntuneBackup
```

### Example 06 - Backup Only Intune Endpoint Security Configurations

```powershell
Invoke-IntuneBackupDeviceManagementIntent -Path C:\temp\IntuneBackup
```

### Example 07 - Restore Only Intune Endpoint Security Configurations

```powershell
Invoke-IntuneRestoreDeviceManagementIntent -Path C:\temp\IntuneBackup
```

### Example 08 - Compare two Backup Files for changes

```powershell
# The DifferenceFilePath should point to the latest Intune Backup file, as it might contain new properties.
Compare-IntuneBackupFile -ReferenceFilePath 'C:\temp\IntuneBackup\Device Configurations\Windows - Endpoint Protection.json' -DifferenceFilePath 'C:\temp\IntuneBackupLatest\Device Configurations\Windows - Endpoint Protection.json'
```

### Example 09 - Compare all files in two Backup Directories for changes

```powershell
# The DifferenceFilePath should point to the latest Intune Backup file, as it might contain new properties.
Compare-IntuneBackupDirectories -ReferenceDirectory 'C:\temp\IntuneBackup' -DifferenceDirectory 'C:\temp\IntuneBackup2'
```

## Known Issues

- Does not support backing up Intune configuration items with duplicate Display Names. Files may be overwritten.
```
