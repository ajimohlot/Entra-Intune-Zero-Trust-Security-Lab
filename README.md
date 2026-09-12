# Microsoft Entra ID & Intune Zero Trust Security Lab

![Microsoft Entra](https://img.shields.io/badge/Microsoft-Entra%20ID-0078D4?style=for-the-badge)
![Microsoft Intune](https://img.shields.io/badge/Microsoft-Intune-0078D4?style=for-the-badge)
![Microsoft Defender](https://img.shields.io/badge/Microsoft-Defender-0078D4?style=for-the-badge)
![Windows 11](https://img.shields.io/badge/Windows-11-0078D4?style=for-the-badge)
![Zero Trust](https://img.shields.io/badge/Security-Zero%20Trust-2EA44F?style=for-the-badge)

## Project Overview

This project documents a hands-on **Microsoft Entra ID and Microsoft Intune Zero Trust Security Lab** where I implemented, tested and troubleshot identity and endpoint security controls in a hybrid Microsoft 365 environment.

The project focused on moving beyond basic administration into security-focused configuration and verification across:

- Microsoft Entra Conditional Access
- Authentication security
- Microsoft Intune Endpoint Security
- Microsoft Defender
- Attack Surface Reduction
- BitLocker device encryption
- Device compliance
- Zero Trust access enforcement

Rather than treating a successfully created policy as proof that a security control was working, I verified configurations through the **Microsoft Entra and Intune portals, real user sign-ins, Windows endpoints, Company Portal, PowerShell, Event Viewer and sign-in logs**.

The final stage brought identity and endpoint security together by using **Intune device compliance as a Conditional Access signal**, demonstrating how access could be granted, blocked and restored based on the security state of the device.

---
## Table of Contents

- [Lab Environment](#lab-environment)
- [Technologies & Skills](#technologies--skills)
- [Project Architecture](#project-architecture)
- [1. Microsoft Entra Conditional Access](#1-microsoft-entra-conditional-access)
- [2. Microsoft Entra Authentication Security](#2-microsoft-entra-authentication-security)
- [3. Microsoft Intune Endpoint Security](#3-microsoft-intune-endpoint-security)
- [4. Microsoft Intune BitLocker & Device Protection](#4-microsoft-intune-bitlocker--device-protection)
- [5. Zero Trust – Conditional Access & Device Compliance](#5-zero-trust--conditional-access--device-compliance)
- [Troubleshooting Highlights](#troubleshooting-highlights)
- [Verification Methodology](#verification-methodology)
- [Video Series](#video-series)
- [Key Takeaways](#key-takeaways)
- [Skills Demonstrated](#skills-demonstrated)
- [Related Project](#related-project)
- [Portfolio](#portfolio)


---

## Lab Environment

| Component | Configuration |
|---|---|
| Microsoft 365 Tenant | Londonbridge Solutions Ltd |
| On-Premises Domain | `ajimohlab.local` |
| Domain Controller | `DC01` |
| Windows Client | `WIN11-CLIENT01` |
| Client OS | Windows 11 Pro |
| Identity | Hybrid Microsoft Entra ID |
| Synchronization | Microsoft Entra Connect |
| Device Management | Microsoft Intune |
| Endpoint Protection | Microsoft Defender |
| Disk Encryption | Microsoft BitLocker |
| Virtualization | Microsoft Hyper-V |

---

## Technologies & Skills

`Microsoft Entra ID` • `Microsoft Intune` • `Conditional Access` • `MFA` • `Temporary Access Pass` • `SSPR` • `Password Writeback` • `Passkeys` • `Authentication Strength` • `Microsoft Defender` • `Windows Firewall` • `Attack Surface Reduction` • `BitLocker` • `TPM` • `Device Compliance` • `Company Portal` • `PowerShell` • `Event Viewer` • `Active Directory` • `Hyper-V` • `Zero Trust`

---

## Project Architecture

```text
                    Microsoft 365
                         │
                  Microsoft Entra ID
                         │
        ┌────────────────┴────────────────┐
        │                                 │
 Authentication Security          Conditional Access
 MFA / TAP / Passkey / SSPR       Identity + Device State
        │                                 │
        └────────────────┬────────────────┘
                         │
                  Microsoft Intune
                         │
          ┌──────────────┴──────────────┐
          │                             │
   Endpoint Security              Device Compliance
 Defender / Firewall / ASR       BitLocker / OS / AV
          │                             │
          └──────────────┬──────────────┘
                         │
                  WIN11-CLIENT01
                         │
                 Zero Trust Access
```

---

# 1. Microsoft Entra Conditional Access

The first stage focused on controlling access to Microsoft 365 resources using **Microsoft Entra Conditional Access**.

Before changing the existing security configuration, I created and verified a cloud-only **Emergency Access Administrator** account and assigned the Global Administrator role.

I then reviewed Security Defaults and the existing Microsoft-managed Conditional Access policies before introducing custom policies.

### What I Configured

- Emergency access administrator account
- Microsoft Entra Security Defaults review
- Microsoft-managed Conditional Access policies
- MFA requirements and exclusions
- **CA01 – Require Compliant Device**
- **CA02 – Block Unsupported Device Platforms**
- Report-only testing
- Conditional Access What If analysis
- Real user sign-in testing
- Entra sign-in log verification

I initially used **Report-only mode and the What If tool** to evaluate policy behaviour before moving to enforcement.

I also performed a real user sign-in and checked the Microsoft Entra sign-in logs rather than relying only on the policy configuration screen.

### Evidence

#### CA01 – Require Compliant Device

![CA01 Compliant Device Configuration](1-Conditional-Access/ZT%2001-12%20CA01_Compliant_Device_Config_Verified.jpg)

#### Conditional Access What If Test

![CA01 What If Verification](1-Conditional-Access/ZT%2001-14%20CA01_WhatIf_Verified.jpg)

#### Real Sign-In Verification

![CA01 Real Sign-In Verification](1-Conditional-Access/ZT%2001-15%20CA01_Real_SignIn_Log_Verified.jpg)

📁 **[View all Conditional Access evidence](1-Conditional-Access)**

### Video Demonstration

▶️ **[Microsoft Entra Conditional Access Lab | MFA, Device Compliance & Access Policies](https://youtu.be/OmqSkXUx29U)**

---

# 2. Microsoft Entra Authentication Security

The second stage focused on strengthening user authentication and account recovery.

I configured and tested:

- Temporary Access Pass
- Microsoft Authenticator
- Self-Service Password Reset
- Password Writeback
- Passkey authentication
- Authentication Strength
- Phishing-resistant MFA

## Temporary Access Pass & Microsoft Authenticator

I created a **Temporary Access Pass (TAP)** for a synchronized user and used it to bootstrap authentication and register Microsoft Authenticator.

This provided a practical example of securely onboarding an authentication method using temporary credentials.

## SSPR & Password Writeback Troubleshooting

I configured Self-Service Password Reset for a selected group and tested password reset using a synchronized Active Directory user.

The initial reset failed because **Password Writeback had not been configured** in the hybrid environment.

I investigated the failure and confirmed that the required on-premises writeback capability was unavailable.

I returned to Microsoft Entra Connect, enabled **Password Writeback**, completed the configuration and verified the writeback integration in Microsoft Entra.

I then repeated the SSPR process and successfully reset the synchronized user's password.

### Troubleshooting Flow

```text
SSPR Attempt
     ↓
Password Reset Failed
     ↓
Investigated On-Premises Integration
     ↓
Password Writeback Not Configured
     ↓
Enabled Password Writeback
     ↓
Verified Writeback Integration
     ↓
Retested SSPR
     ↓
Password Reset Successful
```

## Phishing-Resistant Authentication

I also registered a passkey and configured a Conditional Access policy requiring a **phishing-resistant authentication strength**.

The policy was initially evaluated before enforcement and then enabled for real authentication testing.

I verified the final result through Microsoft Entra sign-in logs.

### Evidence

#### SSPR Password Writeback Failure

![SSPR Password Writeback Error](2-Authentication-Security/ZT%2002-10%20SSPR_Password_Writeback_Error.jpg)

#### Password Writeback Verified in Microsoft Entra

![Password Writeback Entra Verification](2-Authentication-Security/ZT%2002-14%20Password_Writeback_Entra_Verified.jpg)

#### Successful SSPR Reset

![SSPR Reset Success](2-Authentication-Security/ZT%2002-16A%20SSPR_Reset_Success.jpg)

#### Phishing-Resistant Authentication Enforced

![CA03 Enforced Sign-In Success](2-Authentication-Security/ZT%2002-25%20CA03_Enforced_SignIn_Success.jpg)

📁 **[View all Authentication Security evidence](2-Authentication-Security)**

### Video Demonstration

▶️ **[Microsoft Entra Authentication Security Lab | TAP, SSPR & Phishing-Resistant MFA](https://youtu.be/_W7gc3dKLfE)**

---

# 3. Microsoft Intune Endpoint Security

The third stage focused on protecting the Windows 11 endpoint through **Microsoft Intune Endpoint Security**.

Before deploying new controls, I collected a baseline of the existing Microsoft Defender configuration using Windows Security and PowerShell.

## Microsoft Defender Antivirus

I configured Microsoft Defender Antivirus settings through Intune and verified the resulting configuration directly from the endpoint.

Baseline verification included:

```powershell
Get-MpComputerStatus | Select-Object AntivirusEnabled, AntispywareEnabled, RealTimeProtectionEnabled, BehaviourMonitorEnabled

Get-MpPreference | Select-Object PUAProtection, EnableNetworkProtection
```

After policy deployment, I verified:

- Microsoft Defender Antivirus enabled
- Real-time protection enabled
- Behaviour monitoring enabled
- PUA protection enabled
- Network Protection enabled

## Windows Firewall

I configured Windows Firewall through Intune and verified the firewall profiles locally:

```powershell
Get-NetFirewallProfile | Select-Object Name, Enabled
```

The endpoint reported:

```text
Domain     True
Private    True
Public     True
```

This confirmed that all three Windows Firewall profiles were enabled.

## Attack Surface Reduction

I created an Intune **Attack Surface Reduction** policy containing three ASR rules.

The rules were deployed in **Audit mode** so the security controls could initially be evaluated without immediately blocking legitimate activity.

I verified the ASR configuration directly from the Windows endpoint using PowerShell.

The configured rules returned Action `2`, representing **Audit mode**.

I then checked Intune deployment reporting to confirm that the policies had successfully reached the device without errors or conflicts.

### Evidence

#### Defender PowerShell Baseline

![Defender PowerShell Baseline](3-Intune-Endpoint-Security/ZT%2003-03%20Defender_PowerShell_Baseline.jpg)

#### Defender Antivirus Verification

![Defender Antivirus Device Verification](3-Intune-Endpoint-Security/ZT%2003-08%20Defender_Antivirus_Device_Verified.jpg)

#### ASR Verification

![ASR Device Verification](3-Intune-Endpoint-Security/ZT%2003-10%20ASR_Device_Verified.jpg)

#### Endpoint Security Deployment Status

![Endpoint Security Intune Status](3-Intune-Endpoint-Security/ZT%2003-11%20Endpoint_Security_Intune_Status.jpg)

📁 **[View all Intune Endpoint Security evidence](3-Intune-Endpoint-Security)**

### Video Demonstration

▶️ **[Microsoft Intune Endpoint Security Lab | Defender, Firewall & Attack Surface Reduction](https://youtu.be/33vWWfngZKA)**

---

# 4. Microsoft Intune BitLocker & Device Protection

The fourth stage focused on **BitLocker device encryption, TPM readiness, recovery management and troubleshooting**.

I first verified the TPM state:

```powershell
Get-Tpm
```

The virtual TPM was present, enabled, activated and ready.

I then checked the existing BitLocker state:

```powershell
Get-BitLockerVolume

manage-bde -status C:
```

The operating-system drive was already encrypted, but **BitLocker Protection was Off and no key protectors were present**.

## Intune BitLocker Policy

I created an Intune Endpoint Security Disk Encryption policy covering:

- Device encryption
- Recovery password configuration
- Recovery password rotation
- Operating-system drive recovery options
- Recovery information storage
- Encryption-method configuration

The Intune policy successfully reached the Windows 11 endpoint, but **BitLocker Protection remained Off**.

This created a genuine troubleshooting scenario.

## Troubleshooting BitLocker Event ID 853

I first confirmed that the Intune BitLocker settings were present locally:

```powershell
reg query HKLM\SOFTWARE\Microsoft\PolicyManager\Current\device\BitLocker
```

The expected policy values were present.

This established that the problem was **policy enforcement rather than policy delivery**.

I then investigated the BitLocker management logs:

```text
Event Viewer
   ↓
Applications and Services Logs
   ↓
Microsoft
   ↓
Windows
   ↓
BitLocker-API
   ↓
Management
```

Repeated **Event ID 853** entries showed that silent BitLocker enablement had failed.

The detailed event information identified **bootable media attached to the computer**.

I checked the Hyper-V configuration and found that the Windows installation ISO was still mounted in the virtual DVD drive.

### Troubleshooting Flow

```text
Intune BitLocker Policy
          ↓
Policy Reached Endpoint
          ↓
BitLocker Protection Still Off
          ↓
Registry Verification
          ↓
Policy Delivery Confirmed
          ↓
Event Viewer Investigation
          ↓
Event ID 853
          ↓
Bootable Windows ISO Still Mounted
          ↓
ISO Removed
          ↓
WIN11-CLIENT01 Restarted
          ↓
BitLocker Protection On
```

## Verification After Remediation

After removing the installation ISO and restarting the client, I verified:

- TPM remained healthy
- BitLocker Protection changed to **On**
- TPM key protector was present
- Numerical Password protector was present
- Recovery information was available in Microsoft Entra ID
- Recovery information was available in on-premises Active Directory
- Intune reported successful policy deployment

The existing operating-system drive remained:

```text
Encryption Method: XTS-AES 128
Protection Status: Protection On
Key Protectors:
    Numerical Password
    TPM
```

The Intune policy specified XTS-AES 256 for the operating-system drive. However, because the drive had already been encrypted using XTS-AES 128, the existing drive remained XTS-AES 128.

This documentation therefore reflects the **actual verified endpoint state** rather than claiming that the new cipher setting was retroactively applied.

### Evidence

#### BitLocker Protection Still Off

![BitLocker Protection Still Off](4-BitLocker-Device-Protection/ZT%2004-04%20BitLocker_Protection_Still_Off.jpg)

#### Event ID 853 Root Cause

![BitLocker Event 853 Root Cause](4-BitLocker-Device-Protection/ZT%2004-06%20BitLocker_Event853_Root_Cause.jpg)

#### BitLocker Protection On

![BitLocker Protection On](4-BitLocker-Device-Protection/ZT%2004-11%20BitLocker_Protection_On_Verified.jpg)

#### Intune Final Status

![BitLocker Intune Final Status](4-BitLocker-Device-Protection/ZT%2004-14%20BitLocker_Intune_Final_Status.jpg)

> **Security Note:** Sensitive TPM authorization values, BitLocker recovery passwords and recovery-related identifiers were redacted from the public project evidence.

📁 **[View all BitLocker & Device Protection evidence](4-BitLocker-Device-Protection)**

### Video Demonstration

▶️ **[Microsoft Intune BitLocker Lab | Device Encryption, Recovery & Troubleshooting](https://youtu.be/ldwNjqk5wy4)**

---

# 5. Zero Trust – Conditional Access & Device Compliance

The final stage brought the identity and endpoint security controls together in an **end-to-end Zero Trust access scenario**.

I created a Windows compliance policy requiring:

- BitLocker
- Minimum operating-system version
- Windows Firewall
- Antivirus
- Antispyware

`WIN11-CLIENT01` initially satisfied the requirements and reported:

**Compliant**

I then enabled:

**CA01 – Require Compliant Device**

The policy required the accessing device to be marked compliant before access could be granted.

## Stage 1 – Compliant Device: Access Granted

I first verified:

1. `WIN11-CLIENT01` was compliant in Intune.
2. Individual compliance settings passed.
3. CA01 was configured correctly.
4. Conditional Access What If testing produced the expected result.
5. CA01 was enabled.
6. The user successfully accessed the protected resource.
7. Microsoft Entra sign-in evidence confirmed the successful access.

### Evidence

![Compliant Device Baseline](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-03%20WIN11_CLIENT01_Compliant_Baseline.jpg)

![Compliant Device Access Allowed](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-08%20Compliant_Device_Access_Allowed.jpg)

---

## Stage 2 – Controlled Compliance Failure

To test the Zero Trust control, I temporarily raised the required **Minimum OS Version** above the version installed on `WIN11-CLIENT01`.

After Intune reevaluated the device:

```text
Device Status: Noncompliant
```

The per-setting compliance report identified:

```text
Minimum OS Version: Noncompliant
```

Company Portal also reported that the operating system did not meet the organization's compliance requirement.

### Evidence

![Minimum OS Requirement Raised](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-10%20Minimum_OS_Requirement_Raised.jpg)

![Device Noncompliant](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-11%20WIN11_CLIENT01_Noncompliant.jpg)

![Minimum OS Noncompliant](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-12%20Minimum_OS_Version_Noncompliant.jpg)

---

## Stage 3 – Conditional Access Blocks Access

I attempted to access the protected resource again using the same user.

The identity was still valid, but the device no longer satisfied the compliance requirement.

Conditional Access therefore denied access.

The result was verified through:

- Microsoft Intune
- Company Portal
- Microsoft Entra Conditional Access
- Microsoft Entra sign-in evidence

### Evidence

![Grace Access Denied](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-13%20Grace_Access_Denied.jpg)

![CA01 Noncompliant Device Blocked](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-15%20CA01_Noncompliant_Device_Blocked.jpg)

---

## Stage 4 – Remediation & Access Restoration

I restored the correct minimum operating-system requirement and allowed Intune to reevaluate the endpoint.

`WIN11-CLIENT01` returned to:

```text
Compliant
```

I then repeated the user access test and verified that access was restored.

### Evidence

![Compliance Restored](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-17%20WIN11_CLIENT01_Compliance_Restored.jpg)

![Access Restored](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-19%20Grace_Access_Restored.jpg)

![CA01 Compliant Access Restored](5-Zero-Trust-Conditional-Access-Compliance/ZT%2005-21%20CA01_Compliant_Access_Restored.jpg)

### Zero Trust Result

```text
Valid Identity
      +
Compliant Device
      ↓
ACCESS GRANTED


Valid Identity
      +
Noncompliant Device
      ↓
ACCESS BLOCKED


Compliance Remediated
      ↓
ACCESS RESTORED
```

This demonstrated a core **Zero Trust principle**:

> Successful authentication alone does not automatically grant access. The security state of the device can also determine whether access is permitted.

📁 **[View all Zero Trust Conditional Access & Compliance evidence](5-Zero-Trust-Conditional-Access-Compliance)**

### Video Demonstration

▶️ **[Microsoft Entra Zero Trust Lab | Conditional Access & Intune Device Compliance](https://youtu.be/_OTKUEW7_e4)**

---

# Troubleshooting Highlights

A major objective of this project was to demonstrate **investigation, remediation and verification**, rather than only successful policy configuration.

## 1. SSPR Password Writeback Failure

**Problem**

A synchronized user could not complete Self-Service Password Reset.

**Investigation**

Microsoft Entra indicated that Password Writeback was unavailable for the synchronized account.

**Root Cause**

Password Writeback had not been enabled in Microsoft Entra Connect.

**Resolution**

I enabled Password Writeback through Microsoft Entra Connect and verified that the writeback integration was operational.

**Verification**

The synchronized user successfully completed Self-Service Password Reset.

---

## 2. BitLocker Silent Encryption Failure

**Problem**

The Intune BitLocker policy reached the endpoint, but BitLocker Protection remained Off.

**Investigation**

I verified the local MDM policy values and reviewed the BitLocker-API management logs in Event Viewer.

**Root Cause**

BitLocker Event ID 853 identified bootable Windows installation media still attached to the Hyper-V virtual machine.

**Resolution**

I detached the Windows installation ISO and restarted the endpoint.

**Verification**

BitLocker Protection changed to On, key protectors were created, recovery information was available and Intune reported successful policy deployment.

---

## 3. Zero Trust Compliance Failure

**Test Scenario**

I introduced a controlled compliance failure by temporarily raising the Minimum OS Version requirement above the version installed on the Windows 11 endpoint.

**Investigation**

Intune per-setting reporting and Company Portal identified the Minimum OS Version requirement as noncompliant.

**Result**

Conditional Access blocked the user's access because the device was no longer compliant.

**Resolution**

I restored the correct minimum operating-system requirement and allowed Intune to reevaluate the endpoint.

**Verification**

The device returned to Compliant and Conditional Access restored the user's access.

---

# Verification Methodology

Throughout the project, I followed a consistent validation process:

```text
Baseline
   ↓
Configure
   ↓
Apply / Sync
   ↓
Verify in Portal
   ↓
Verify on Endpoint / User
   ↓
Review Logs / Status
   ↓
Capture Evidence
```

This meant that a successful configuration or deployment status was **not treated as proof that the security control was actually functioning**.

Where applicable, I verified changes through:

- Microsoft Intune reporting
- Microsoft Entra sign-in logs
- Conditional Access What If
- Real user sign-ins
- Company Portal
- Windows Security
- PowerShell
- Windows Event Viewer
- Active Directory Users and Computers
- Endpoint policy state

---

# Video Series

| Part | Topic | Video |
|---|---|---|
| 1 | Microsoft Entra Conditional Access | [Watch Video](https://youtu.be/OmqSkXUx29U) |
| 2 | Microsoft Entra Authentication Security | [Watch Video](https://youtu.be/_W7gc3dKLfE) |
| 3 | Microsoft Intune Endpoint Security | [Watch Video](https://youtu.be/33vWWfngZKA) |
| 4 | Microsoft Intune BitLocker & Device Protection | [Watch Video](https://youtu.be/ldwNjqk5wy4) |
| 5 | Zero Trust Conditional Access & Device Compliance | [Watch Video](https://youtu.be/_OTKUEW7_e4) |

---

# Key Takeaways

This project strengthened my practical understanding of how Microsoft identity and endpoint security technologies work together.

Key lessons from the project included:

- Conditional Access decisions can incorporate both identity and device state.
- Emergency access accounts should be considered before introducing restrictive access policies.
- Report-only mode and What If testing provide useful validation before policy enforcement.
- Authentication security extends beyond traditional password-based MFA.
- Temporary Access Pass can support secure authentication onboarding.
- Hybrid SSPR depends on correctly configured Password Writeback.
- Phishing-resistant authentication can be enforced using authentication strengths and Conditional Access.
- Intune deployment status should be supported by direct endpoint verification.
- PowerShell provides valuable evidence when graphical interfaces do not clearly reflect endpoint state.
- Event Viewer can expose the root cause when a policy reaches a device but does not produce the expected behaviour.
- BitLocker verification should include encryption state, protection state, key protectors and recovery information.
- Device compliance can function as a real access-control signal.
- Zero Trust combines identity, device state and policy rather than trusting access solely because valid credentials were supplied.

---

# Skills Demonstrated

### Identity & Access Management

- Microsoft Entra ID administration
- Conditional Access
- MFA
- Emergency access administration
- Authentication methods
- Temporary Access Pass
- Self-Service Password Reset
- Password Writeback
- Passkey authentication
- Authentication Strength
- Sign-in log analysis

### Endpoint Security

- Microsoft Intune
- Microsoft Defender Antivirus
- Windows Defender Firewall
- Attack Surface Reduction
- Endpoint policy deployment
- Endpoint policy verification
- Device compliance

### Device Protection

- BitLocker
- TPM
- Recovery-key management
- Microsoft Entra recovery information
- Active Directory recovery information
- BitLocker troubleshooting

### Troubleshooting & Verification

- PowerShell
- Windows Event Viewer
- Intune reporting
- Microsoft Entra sign-in logs
- Company Portal
- Registry policy verification
- Root-cause analysis
- Policy remediation
- End-user verification

---

# Related Project

This security lab builds on the hybrid Microsoft 365 environment created in my earlier project:

**[Microsoft 365 Administration, Entra ID & Intune Home Lab](https://github.com/ajimohlot/Microsoft-365-Administration)**

That project covered the underlying Microsoft 365 tenant, Active Directory, Microsoft Entra Connect, hybrid identity, Intune enrollment, user administration and troubleshooting used as the foundation for this security-focused lab.

---

# Portfolio

This project forms part of my hands-on **IT infrastructure, Microsoft 365, endpoint management and cybersecurity portfolio**.

Additional projects are available on my GitHub profile:

**[github.com/ajimohlot](https://github.com/ajimohlot)**

The accompanying video demonstrations provide end-to-end evidence of the configuration, testing, troubleshooting and verification performed throughout the project.
