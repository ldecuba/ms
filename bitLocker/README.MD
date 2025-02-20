# BitLocker Migration Guide
*From On-premises to Azure/Entra ID*

**Version:** 1.0  
**Last Updated:** February 2025  
**Author:** Lisandro de Cuba  

---

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

---

## Prerequisites

### Required Licenses and Components
1. Azure/Entra ID Premium P1 or P2 license
2. Devices must be Azure AD joined or Hybrid Azure AD joined
3. TPM 2.0 enabled on target devices
4. MBAM (Microsoft BitLocker Administration and Monitoring) current version if migrating from MBAM

---

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

---

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

---

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

---

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

---

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

---

## Troubleshooting Guide

### 1. TPM Activation Issues
#### Common Symptoms
- TPM not detected
- TPM initialization failures
- TPM ownership errors

#### Resolution Steps
- Verify TPM status in BIOS/UEFI
- Clear and reinitialize TPM
- Update TPM firmware

#### References
- [Microsoft TPM Troubleshooting Guide](https://learn.microsoft.com/en-us/windows/security/information-protection/tpm/troubleshoot-tpm)
- [TPM Ready Status](https://learn.microsoft.com/en-us/windows/security/information-protection/tpm/initialize-and-configure-tpm)

### 2. Network Connectivity Issues
#### Common Symptoms
- Failed key backup to Azure AD
- Device sync issues
- Certificate errors

#### Resolution Steps
- Test Azure AD connectivity
- Verify proxy settings
- Check network certificates

#### References
- [Azure AD Connect Health](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-operations)
- [Network Requirements for Intune](https://learn.microsoft.com/en-us/mem/intune/fundamentals/network-bandwidth-use)

### 3. Permission and Access Issues
#### Common Symptoms
- Access denied errors
- Failed policy application
- Key backup failures

#### Resolution Steps
- Verify Azure AD roles
- Check group memberships
- Review conditional access policies

#### References
- [Azure AD Role Documentation](https://learn.microsoft.com/en-us/azure/active-directory/roles/permissions-reference)
- [BitLocker CSP Documentation](https://learn.microsoft.com/en-us/windows/client-management/mdm/bitlocker-csp)

### 4. Group Policy Conflicts
#### Common Symptoms
- Conflicting encryption settings
- Policy application failures
- Inconsistent behavior

#### Resolution Steps
- Run GPResult reports
- Review policy precedence
- Check for conflicting MDM policies

#### References
- [Group Policy for BitLocker](https://learn.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings)
- [MDM and Group Policy Interaction](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-configuration-service-provider)

### 5. Recovery Procedures
#### Emergency Recovery Steps
1. Access Azure Portal BitLocker section
2. Locate device recovery key
3. Document recovery attempt
4. Verify successful unlock

#### References
- [BitLocker Recovery Guide](https://learn.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-recovery-guide-plan)
- [Azure AD BitLocker Recovery](https://learn.microsoft.com/en-us/azure/active-directory/devices/bitlocker-recovery)

---

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

---

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
| 1.0 | 2025-02-20 | Lisandro de Cuba | Initial documentation |

### Related Documents
- BitLocker Security Policy
- Azure AD Configuration Guide
- Emergency Recovery Procedures
- Compliance Requirements Document

### Support Contacts
- IT Security Team: security@company.com
- Azure Support: azure-support@company.com
- Help Desk: helpdesk@company.com

---

*Note: This document should be reviewed and updated quarterly or when significant changes occur in the environment.*

*Last reviewed: February 20, 2025*
