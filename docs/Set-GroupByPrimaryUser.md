# Set-GroupByPrimaryUser

Synchronize a **device-based security group** with the devices owned by the **primary users** of a **user group** (including nested users).  
The function adds missing devices, optionally removes devices that no longer qualify, and supports filtering by OS and ownership.

> **Use case:** Keep an Intune device group aligned with a user group’s primary users for deployments, compliance, or reporting.

---

## Features

- Pulls all users from a source **user group** (transitive—nested groups supported).
- Resolves each user’s **owned devices** (i.e., where they are set as **Primary User**).
- Adds missing devices to the target **device group**.
- Optionally removes devices that no longer belong (`-noRemove` to skip removals).
- Filters by **Operating System** (`Windows`, `MacMDM`, `iOS`, `Android`, `Linux`) and/or **Ownership** (`Company`, `Personal`).
- De-duplicates device IDs.
- Auto-installs required **Microsoft Graph PowerShell** modules if missing.

---

## Requirements

- **PowerShell:** 5.1+ (Windows) or 7+ (Core)  
- **Modules:**  
  - `Microsoft.Graph.Users`  
  - `Microsoft.Graph.Groups`  
  - `Microsoft.Graph.DeviceManagement`  
  *(The function installs/imports these if needed.)*
- **Group Types:** The **device target group must be a *security group* with Assigned membership** (not Dynamic). The user source group can be security or M365; nested groups are supported via transitive membership.
- **Intune/Entra configuration:** Devices must correctly reflect **Primary User** in Intune/Entra.
- **Permissions/Roles (delegated example):** You (or your app) need Graph scopes capable of reading users/devices and **writing group membership**, e.g.:
  - `Group.ReadWrite.All`
  - `User.Read.All`
  - `Device.Read.All`

  Connect like:
  ```powershell
  Connect-MgGraph -Scopes "Group.ReadWrite.All","User.Read.All","Device.Read.All","Directory.Read.All"
  ```

> In automated scenarios (e.g., Azure Automation with Managed Identity), grant **Application** permissions (app roles) equivalent to the above.



## Usage

```powershell
Set-GroupByPrimaryUser `
  -UserGroupObjectID  "<SOURCE-USER-GROUP-OBJECT-ID>" `
  -DeviceGroupObjectID "<TARGET-DEVICE-GROUP-OBJECT-ID>" `
  [-OSFilter Windows|MacMDM|iOS|Android|Linux ...] `
  [-OwnershipFilter Company|Personal] `
  [-noRemove]
```

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `UserGroupObjectID` | `string` | Yes | Object ID of the **user** group (source). Transitive members (nested) are included. |
| `DeviceGroupObjectID` | `string` | Yes | Object ID of the **device** security group (target, Assigned membership). |
| `OSFilter` | `string[]` | No | Only include devices with an OS in this set: `Windows`, `MacMDM`, `iOS`, `Android`, `Linux`. |
| `OwnershipFilter` | `string` | No | Only include devices with ownership `Company` or `Personal`. |
| `noRemove` | `switch` | No | If set, the function **adds** missing devices but **does not remove** any existing members. |

---

## Examples

**1) Basic sync (add & remove):**
```powershell
Connect-MgGraph -Scopes "Group.ReadWrite.All","User.Read.All","Device.Read.All","Directory.Read.All"

Set-GroupByPrimaryUser `
  -UserGroupObjectID  "11111111-1111-1111-1111-111111111111" `
  -DeviceGroupObjectID "22222222-2222-2222-2222-222222222222"
```

**2) Windows-only, Company-owned:**
```powershell
Set-GroupByPrimaryUser `
  -UserGroupObjectID  "11111111-1111-1111-1111-111111111111" `
  -DeviceGroupObjectID "22222222-2222-2222-2222-222222222222" `
  -OSFilter Windows `
  -OwnershipFilter Company
```

**3) Mac + iOS, add-only (safe mode):**
```powershell
Set-GroupByPrimaryUser `
  -UserGroupObjectID  "11111111-1111-1111-1111-111111111111" `
  -DeviceGroupObjectID "22222222-2222-2222-2222-222222222222" `
  -OSFilter MacMDM,iOS `
  -noRemove
```

**4) Multiple OS filters:**
```powershell
Set-GroupByPrimaryUser `
  -UserGroupObjectID  "11111111-1111-1111-1111-111111111111" `
  -DeviceGroupObjectID "22222222-2222-2222-2222-222222222222" `
  -OSFilter Windows,Android
```

---

## How it works (high level)

1. Resolve **all users** from the source group (transitive).
2. For each user, get **owned devices** (Primary User relationship).
3. Optionally filter by **OS** and/or **Ownership**.
4. **Add** any missing device members to the target device group.
5. Unless `-noRemove` is specified, **remove** devices from the target group that are not in the computed set.

---

## Notes & Recommendations

- **Primary User accuracy** is critical. If devices are missing, verify their Primary User in Intune and that the device exists in Entra.
- **Dynamic device groups** cannot be directly modified-use an **Assigned** security group for the device target.
- **Rate limits:** For large groups, you may encounter Graph throttling. Re-run if needed or schedule during off-hours.
- **Ownership values** depend on how your environment sets `DeviceOwnership` (`Company` vs `Personal`).
- **Authentication:** The function snippet shows a placeholder for authentication. Make sure you call `Connect-MgGraph` before running the function (see examples).

---

## Troubleshooting

- **Devices not added:**  
  - Check that the device exists in Entra and the user is set as **Primary User**.  
  - Confirm OS/ownership filters arent excluding it.  
  - Ensure the target group is a **device security group** with **Assigned** membership.
- **"Insufficient privileges" errors:**  
  - Reconnect with proper scopes (`Group.ReadWrite.All`, etc.) or use an app with appropriate **Application** permissions.
- **Module import/installation issues:**  
  - Update to the latest `Microsoft.Graph` modules:  
    ```powershell
    Update-Module Microsoft.Graph -Scope CurrentUser
    ```


## Author

- **Florian Salzmann** – Microsoft MVP  
  - X/Twitter: **@FlorianSLZ**

