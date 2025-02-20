# BitLocker Migration Guide

*From On-premises to Azure/Entra ID*



**Version:** 1.0  

**Last Updated:** February 2025  

**Author:** Lisandro de Cuba  



## Table of Contents

- [Prerequisites](#prerequisites)

- [Required Permissions](#required-permissions)

- [Pre-Migration Steps](#pre-migration-steps)

- [Migration Steps](#migration-steps)

- [Post-Migration Tasks](#post-migration-tasks)

- [PowerShell Commands](#powershell-commands-for-migration)

- [Troubleshooting Guide](#troubleshooting-guide)

- [Monitoring and Compliance](#monitoring-and-compliance)

- [Security Considerations](#security-considerations)



## Prerequisites



### Required Licenses and Components

1. Azure/Entra ID Premium P1 or P2 license

2. Devices must be Azure AD joined or Hybrid Azure AD joined

3. TPM 2.0 enabled on target devices

4. MBAM (Microsoft BitLocker Administration and Monitoring) current version if migrating from MBAM



## Required Permissions



### On-Premises Environment

- Domain Administrator or equivalent permissions to modify Group Policy

- MBAM Administrator role (if migrating from MBAM)

- Local Administrator rights on client machines

- Backup Operator rights for BitLocker recovery keys



### Azure/Entra ID Environment

- Global Administrator or Security Administrator role

- Intune Administrator role

- Azure Information Protection Administrator role

- Cloud Device Administrator role



## Pre-Migration Steps



### 1. Inventory Current Environment

- Document all BitLocker-encrypted devices

- Export current BitLocker recovery keys

- Document existing Group Policy settings

- Identify any custom scripts or automation



### 2. Azure/Entra ID Configuration

- Enable Azure Information Protection

- Configure Intune endpoint protection policies

- Set up Azure AD groups for BitLocker management



### 3. Backup Current Environment

- Export BitLocker recovery keys from AD DS

- Backup Group Policy Objects (GPOs)

- Document MBAM configuration (if applicable)



## Migration Steps



### 1. Device Migration Preparation

```powershell

# Export current BitLocker recovery information

manage-bde -protectors -get C: > bitlocker_backup.txt

```



### 2. Azure/Entra ID Configuration

- Enable BitLocker management in Endpoint Manager

- Configure encryption settings

- Set up monitoring and reporting



### 3. Group Policy Updates

- Create new GPO for transition period

- Configure following settings:

```

Computer Configuration\Administrative Templates\Windows Components\BitLocker Drive Encryption

- Enable "Store BitLocker recovery information in Azure AD"

- Configure "Choose how BitLocker-protected operating system drives can be recovered"

```



### 4. Staged Migration Process

a. Pilot Group (10% of devices)

b. Validation Period (1-2 weeks)

c. Full Deployment (remaining devices)



## Post-Migration Tasks



### 1. Verification Steps

- Check BitLocker status in Endpoint Manager

- Verify recovery key backup in Azure AD

- Test recovery process

- Monitor encryption status



### 2. Clean-up Tasks

- Remove old GPOs

- Archive MBAM database

- Update documentation

- Remove legacy permissions



### 3. Ongoing Management

- Configure alerting

- Set up reporting

- Document new processes

- Train support staff



## PowerShell Commands for Migration



```powershell

# Check BitLocker status

$BitLockerVolume = Get-BitLockerVolume -MountPoint "C:"

$BitLockerVolume.ProtectionStatus



# Backup recovery key to Azure AD

BackupToAAD-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId $BitLockerVolume.KeyProtector[1].KeyProtectorId



# Verify Azure AD backup

Get-BitLockerVolume C: | Select-Object *

```



## Troubleshooting Guide



### 1. Common Issues

- TPM activation failures

- Network connectivity issues

- Permission errors

- Group Policy conflicts



### 2. Resolution Steps

- Check device compliance status

- Verify network connectivity to Azure AD

- Review error logs

- Test BitLocker protectors



### 3. Emergency Recovery Procedures

- Document offline recovery process

- Maintain backup of recovery keys

- Create emergency access process



## Monitoring and Compliance



### 1. Regular Checks

- Device encryption status

- Recovery key backup status

- Policy compliance

- Failed encryption attempts



### 2. Reporting Requirements

- Weekly compliance reports

- Monthly status updates

- Quarterly audit reviews

- Annual security assessments



## Security Considerations



### 1. Data Protection

- Maintain secure backup of recovery keys

- Implement least privilege access

- Monitor recovery key access

- Regular security audits



### 2. Compliance Requirements

- Document regulatory compliance

- Maintain audit logs

- Review access policies

- Update security procedures



---



## Document Information



### Change Log

| Version | Date | Author | Changes |

|---------|------|--------|----------|

| 1.0 | 2024-03-11 | Lisandro de Cuba | Initial documentation |








*Note: This document should be reviewed and updated quarterly or when significant changes occur in the environment.*


