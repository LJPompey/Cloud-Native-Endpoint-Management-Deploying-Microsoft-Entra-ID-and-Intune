<div align="center">
  
# ☁️ Cloud-Native-Endpoint-Management-Deploying-Microsoft-Entra-ID-and-Intune

This repository documents the deployment of a cloud-managed enterprise environment using Microsoft Entra ID and Microsoft Intune. Built as an extension to my local Automated SOC Pipeline (Wazuh, Sysmon, and TheHive), this project demonstrates modern unified endpoint management (UEM), automated zero-touch application deployment, and zero-trust hardware security enforcement on a Windows 11 virtual machine.

[Read the Full Technical Breakdown on Medium](https://medium.com/@pompey.lamont01/bridging-the-gap-deploying-a-cloud-native-endpoint-management-lab-with-microsoft-intune-3c6229b57e1e)

</div>

---

## 🖥️ Technologies & Infrastructure

### 🪪 Identity Provider: Microsoft Entra ID

* MDM / UEM: Microsoft Intune
* Hybrid Sync: Azure AD Connect
* Target Endpoint: Windows 11 Pro (VMware/VirtualBox via NAT)
* Deployment Tools: Microsoft Win32 Content Prep Tool (.intunewin)

### 📊 Project Architecture

* Identity Synchronization: Local Active Directory users are continuously synced to Entra ID via Azure AD Connect.
* Endpoint Enrollment: A standalone Windows 11 Workgroup VM joins Entra ID directly, triggering automatic Intune MDM enrollment.
* Application Delivery: Google Chrome is packaged and pushed silently to the endpoint from the cloud.
* Security Enforcement: A device-level administrative template restricts USB storage access to prevent data exfiltration.

### 🪜 Implementation Steps

* Phase 1: Environment Preparation: Provisioned a Microsoft 365 E3 trial tenant and established hybrid identity synchronization. Assigned E3 licensing to synchronized user accounts to enable Intune enrollment capabilities.
* Phase 2: Entra ID Join: Configured a Windows 11 endpoint on a NAT network and joined it directly to Entra ID, verifying automatic MDM enrollment scopes.
* Phase 3: Zero-Trust Hardware Policy: Utilized the Intune Settings Catalog to deploy a Removable Disks: Deny read access configuration profile.
* Phase 4: Win32 App Deployment: Packaged the Google Chrome Standalone Enterprise .msi installer into an .intunewin file using the command line. Configured Intune detection rules using the MSI product code for silent background installation.

### ⚙️ Troubleshooting & Resolutions

* 400 Bad Request on Sync: Endpoint rejected Intune synchronization due to a missing cloud license on the synced user account. Fix: Assigned the M365 E3 license, forced a complete sign-out on the endpoint to flush the cached token, and successfully pulled a fresh authentication token upon sign-in.
* USB Block Policy Failure (User vs. Device): Initial deployment of the USB block policy failed to enforce at the hardware level when assigned to user groups. Fix: Pivoted the assignment target from "All Users" to "All Devices" to grant the policy the necessary system-level execution rights to lock down the storage controllers.
