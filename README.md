# Hybrid Active Directory, Microsoft Entra ID & Intune Deployment

## Project Overview
Designed and configured a hybrid identity and endpoint management environment integrating on-premises Active Directory with Microsoft Entra ID and Microsoft Intune.

The project demonstrates practical experience with identity synchronization, Windows device management, Group Policy, and endpoint security.

## Technologies Used
- Windows Server 2022
- Active Directory Domain Services (AD DS)
- DNS and Group Policy
- Microsoft Entra Connect Sync
- Microsoft Entra ID
- Microsoft Intune
- Windows 11 Enterprise
- VMware Workstation

## Implementation
- Configured Active Directory and DNS services.
- Implemented identity synchronization between on-premises AD and Microsoft Entra ID.
- Configured hybrid Microsoft Entra join for a Windows 11 Enterprise device.
- Enabled automatic Intune enrollment using Group Policy.
- Created and assigned an Intune device configuration profile.
- Deployed Microsoft Defender SmartScreen settings.
- Configured and verified a Windows Firewall compliance policy.

## Validation
- Verified hybrid device join using `dsregcmd /status`.
- Confirmed Microsoft Defender SmartScreen enforcement through `edge://policy`.
- Verified Windows Firewall status using PowerShell.
- Confirmed successful device enrollment and compliance reporting in Microsoft Intune.

## Project Status
**Completed:** Hybrid identity synchronization, device enrollment, security configuration, and compliance verification.

**Planned:** BitLocker encryption management, recovery-key escrow, and additional endpoint security configurations.

## Purpose
Demonstrate hands-on technical experience with hybrid Microsoft infrastructure, endpoint administration, and security management relevant to enterprise IT support and systems administration.

## Hybrid Join Verification

The following screenshot confirms successful hybrid Microsoft Entra join, TPM protection, and cloud device authentication.

![Hybrid Join Verification](screenshots/hybrid-join-verification.png)

## Intune Enrollment and Compliance Verification

The Windows 11 Enterprise endpoint was successfully enrolled in Microsoft Intune and classified as a corporate-managed device.

The Intune admin center confirms:
- Device compliance: **Compliant**
- Device ownership: **Corporate**
- Management platform: **Microsoft Intune**
- Configuration policies: **1 succeeded**
- Compliance policies: **2 compliant**

![Intune Enrollment and Compliance Verification](screenshots/intune-enrollment-compliance.png)

## Microsoft Edge SmartScreen Policy Verification

Deployed a Microsoft Edge SmartScreen security configuration
through Microsoft Intune and verified enforcement on the
Windows 11 Enterprise endpoint.

**Verification results:**
- Policy: SmartScreenEnabled
- Value: true
- Source: Platform
- Applies to: Device
- Level: Mandatory
- Status: OK

![Edge SmartScreen Policy Verification](screenshots/edge-smartscreen-policy.png)

## Windows Firewall Compliance Verification

Configured a Microsoft Intune compliance policy requiring
Windows Firewall to be enabled.

Verified the endpoint firewall configuration using PowerShell.

**Results:**
- Domain: Enabled
- Private: Enabled
- Public: Enabled
- Intune compliance policy: Compliant

![Windows Firewall Verification](screenshots/windows-firewall-verification.png)

# BitLocker Encryption and Recovery Key Management

## Project Overview

Implemented and verified BitLocker drive encryption on a hybrid Microsoft Entra-joined Windows 11 Enterprise virtual machine managed through Microsoft Intune.

Configured endpoint encryption policies, TPM-based protection, and Active Directory Domain Services (AD DS) recovery password storage. Troubleshot encryption deployment and verified device encryption status through Windows PowerShell, Active Directory, and the Microsoft Intune admin center.

## Environment

| Component | Configuration |
|---|---|
| Client OS | Windows 11 Enterprise |
| Server OS | Windows Server 2022 |
| Virtualization | VMware Workstation Pro |
| Identity | Active Directory Domain Services and Microsoft Entra ID |
| Device management | Microsoft Intune |
| Device join | Hybrid Microsoft Entra joined |
| Encryption | BitLocker XTS-AES 256-bit |
| Hardware security | Virtual TPM 2.0 |
| Recovery management | Active Directory Domain Services |

## Implementation

### 1. Configured Microsoft Intune BitLocker Policy

Created and assigned an endpoint security disk encryption policy:

**Policy:** `WIN11 - BitLocker Encryption - Test`

Configuration included:

- Require device encryption
- XTS-AES 256-bit encryption for operating system drives
- Used-space-only encryption
- TPM-based operating system drive protection
- Require a 48-digit recovery password
- Store recovery information in Active Directory Domain Services
- Require AD DS recovery information storage before enabling encryption
- Recovery password rotation policy for Microsoft Entra-joined and hybrid-joined devices

Assigned the policy to the `Intune-Test-Devices` group containing the Windows 11 Enterprise VM.

### 2. Verified Policy Deployment

Confirmed that the device received its BitLocker configuration through Microsoft Intune.

Validated policy settings through the Windows registry and monitored Intune device check-in and configuration policy status.

The BitLocker policy eventually reported successful deployment.

### 3. Troubleshot Encryption Initialization

Although Intune successfully delivered the configuration, the operating system drive initially remained unencrypted.

Investigated the issue using:

- `Get-BitLockerVolume`
- `manage-bde -status`
- Windows Event Viewer
- MDM policy registry settings
- Group Policy Results (`gpresult`)
- TPM readiness checks

Confirmed that the device's TPM was operational and that the BitLocker configuration had been received.

Because automatic encryption had not started, completed encryption initialization manually while retaining the configured security requirements.

### 4. Configured Recovery Password Protection

Verified TPM key protection and created a BitLocker recovery password protector.

Backed up recovery password information to Active Directory Domain Services.

Used the BitLocker Recovery Password Viewer in Active Directory Users and Computers to verify successful recovery information storage.

### 5. Enabled Operating System Drive Encryption

Initiated used-space-only encryption using the configured XTS-AES 256-bit algorithm.

Monitored encryption progress and confirmed successful completion.

**Final local verification:**

| Setting | Result |
|---|---|
| Operating system drive | C: |
| Encryption progress | 100% |
| Encryption method | XTS-AES 256 |
| Conversion status | Used Space Only Encrypted |
| Protection status | On |
| Key protectors | TPM and Recovery Password |

### 6. Verified Recovery Information in Active Directory

Located the Windows 11 computer object in Active Directory Users and Computers.

Confirmed that BitLocker recovery information was stored under the computer object and accessible through the BitLocker Recovery tab.

Verified the active recovery password protector against its corresponding AD DS recovery object.

Removed an obsolete recovery information object after confirming the active protector and its associated recovery information.

### 7. Verified Microsoft Intune Encryption Reporting

Following encryption completion, Microsoft Intune initially reported the device as unencrypted.

Performed a device synchronization and allowed updated device information to be reported.

**Final Intune results:**

- Device enrollment: Successful
- Configuration policies: Succeeded
- Device compliance: Compliant
- Device encryption status: **Yes**
- Last verified check-in: November 5, 2026, 3:53 PM

## Troubleshooting Experience

This implementation involved diagnosing several configuration and reporting issues:

- BitLocker policy applied without automatically initiating encryption
- Differences between successful policy deployment and actual drive encryption
- Recovery password protector creation and verification
- Active Directory recovery password backup and object management
- Delayed encryption status reporting in Microsoft Intune

Resolved these issues through PowerShell diagnostics, local BitLocker management, Active Directory administration, and Intune device synchronization.

## Outcome

Successfully encrypted and secured a hybrid Microsoft Entra-joined Windows 11 Enterprise endpoint using BitLocker and Microsoft Intune policy configuration.

Verified TPM-based protection, AD DS recovery password storage, successful encryption, and accurate Intune device reporting.

**Technical skills demonstrated:** Microsoft Intune, BitLocker, Microsoft Entra ID, Active Directory Domain Services, Windows Server 2022, Windows 11 Enterprise, PowerShell, Group Policy troubleshooting, TPM 2.0, endpoint encryption, recovery key management, and device compliance monitoring.

## Security Considerations

Recovery passwords and other sensitive authentication information are excluded from public documentation. Screenshots should conceal recovery passwords, tenant identifiers, and other sensitive environment details.

**Implementation completed:** November 5, 2026.
