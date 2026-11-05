# Hybrid Active Directory, Entra ID & Intune — Technical Implementation

## 1. Project Objective

Deploy and validate a hybrid Microsoft identity and endpoint management environment connecting Windows Server 2022 Active Directory with Microsoft Entra ID and Microsoft Intune.

The objective was to demonstrate identity synchronization, hybrid device registration, automatic MDM enrollment, configuration policy deployment, and endpoint compliance monitoring.

## 2. Environment and Infrastructure

| Component | Configuration |
|---|---|
| Hypervisor | VMware Workstation |
| Domain Controller | Windows Server 2022 Standard Evaluation |
| Directory Services | Active Directory Domain Services |
| Network Services | DNS and Group Policy |
| Cloud Identity | Microsoft Entra ID |
| Synchronization | Microsoft Entra Connect Sync |
| Endpoint Management | Microsoft Intune |
| Client Operating System | Windows 11 Enterprise |
| Licensing | Microsoft 365 Business Premium trial |

The Windows Server and Windows 11 Enterprise virtual machines operated within a VMware NAT network.

## 3. Active Directory and Identity Synchronization

Configured Active Directory Domain Services and DNS on Windows Server 2022.

Installed and configured Microsoft Entra Connect Sync to integrate on-premises directory identities with Microsoft Entra ID.

Implementation included:

- Configuring password hash synchronization.
- Creating dedicated organizational units for synchronized users and devices.
- Limiting synchronization scope to designated organizational units.
- Synchronizing a test user account to Microsoft Entra ID.
- Verifying the synchronized identity in the Microsoft Entra administration portal.

**Result:** The on-premises test identity was successfully synchronized to Microsoft Entra ID.

## 4. Hybrid Microsoft Entra Join

Configured hybrid device registration for a domain-joined Windows 11 Enterprise virtual machine.

Implementation included:

- Configuring the Service Connection Point (SCP) through Microsoft Entra Connect.
- Including the designated computer organizational unit in synchronization scope.
- Verifying the Windows 11 device appeared in Microsoft Entra ID.
- Validating the device registration state using PowerShell and Windows command-line tools.

**Validation command:**

`dsregcmd /status`

Verified:

- `AzureAdJoined : YES`
- `DomainJoined : YES`
- `DeviceAuthStatus : SUCCESS`

**Result:** The Windows 11 Enterprise device was successfully hybrid joined.

## 5. Automatic Intune Enrollment

Configured Group Policy to automatically enroll the hybrid-joined Windows 11 Enterprise device into Microsoft Intune.

Implementation included:

- Configuring the Microsoft Intune MDM user scope.
- Creating and assigning a dedicated Entra security group.
- Configuring Group Policy for automatic MDM enrollment using user credentials.
- Applying the policy to the designated Active Directory computer organizational unit.
- Signing in with the synchronized domain user account.
- Verifying successful Intune enrollment.

**Validation commands:**

`gpupdate /force`

`gpresult /scope computer /r`

**Result:** The Windows 11 Enterprise device enrolled successfully into Microsoft Intune and appeared as a corporate-managed endpoint.

## 6. Endpoint Security Configuration

Created a Windows configuration profile
