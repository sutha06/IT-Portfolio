#2.1 Lab: Managing System Services in Windows
**Course:** System Administration and IT Infrastructure Services
---
## Lab Overview
The objective of this hands-on lab was to demonstrate essential **system administration skills** in a Windows Server environment. The core tasks involved controlling services via both GUI and PowerShell command-line interface, enabling system features, and configuring web services. This showcases foundational abilities to maintain and manage Windows server infrastructure.

## Skills Demonstrated
* **Service Management via GUI:** Using the Services management console to start, stop, and configure Windows services.
* **PowerShell Proficiency:** Managing services through command-line using `Get-Service`, `Start-Service`, `Stop-Service`, and `Set-Service`.
* **Windows Feature Installation:** Enabling additional system features using `Install-WindowsFeature`.
* **Web Server Configuration:** Installing and configuring Internet Information Services (IIS) to serve web pages.
* **Service Troubleshooting:** Diagnosing disabled services and modifying startup types to enable functionality.

---

## Step-by-Step Execution

### 1. Introduction to PowerShell Service Management
The lab began with learning how to interact with Windows services through PowerShell rather than relying solely on GUI tools, which is essential for automation and remote administration.

**Opening PowerShell:**
![Screenshot: Opening Windows PowerShell from the Start menu](assets/01_powershell_start.png)

**Listing All Services:**
```powershell
Get-Service
```

This command displays all available services with their status, short name, and display name. The output includes hundreds of services, both running and stopped.

### 2. Examining a Specific Service
To get detailed information about a specific service, the lab focused on the Windows Insider Service (`wisvc`).

| Action | Command Used | Key Findings |
| :--- | :--- | :--- |
| **Basic Status** | `Get-Service wisvc` | Service was stopped |
| **Detailed Information** | `Get-Service wisvc \| Format-List *` | Revealed startup type, dependencies, and capabilities |

**Key output fields:**
- `Status`: Stopped
- `StartType`: Manual
- `ServicesDependedOn`: rpcss (Remote Procedure Call)

### 3. Starting and Stopping Services
Demonstrated the ability to control service states using PowerShell commands.

```powershell
# Start the Windows Insider Service
Start-Service wisvc

# Verify the service is running
Get-Service wisvc

# Stop the service
Stop-Service wisvc

# Confirm it's stopped
Get-Service wisvc
```

**Verification Process:**
![Screenshot: PowerShell showing service status changes](assets/02_service_control.png)

### 4. Enabling Disabled Services
This section demonstrated how to work with services that are disabled and cannot be started until their startup type is changed.

**Target Service:** Smart Card service (`ScardSvr`)

1. **Diagnosis:** Used `Get-Service ScardSvr | Format-List *` to identify that `StartType: Disabled`
2. **Attempted Start (Failed):**
   ```powershell
   Start-Service ScardSvr
   ```
   This produced an error: `Cannot start service ScardSvr on computer '.'`

3. **Enable the Service:**
   ```powershell
   Set-Service ScardSvr -StartupType Manual
   ```

4. **Successful Start:**
   ```powershell
   Start-Service ScardSvr
   Get-Service ScardSvr
   ```
   The service was now running successfully.

![Screenshot: Enabling and starting the ScardSvr service](assets/03_enable_service.png)

### 5. Installing Windows Features
Demonstrated the installation of additional Windows features that are not enabled by default.

**Installing IIS Web Server:**
```powershell
Install-WindowsFeature Web-WebServer,Web-Mgmt-Tools -IncludeAllSubFeature
```

This command:
- Downloads required components
- Installs the web server role and management tools
- Includes all sub-features automatically
- Takes several minutes to complete

**Output confirmation:**
```
Success Restart Needed Exit Code      Feature Result
------- -------------- ---------      --------------
True    No             Success        {.NET Framework 3.5 (includes .NET 2.0 and...
```

**Verifying IIS Service:**
```powershell
Get-Service IISADMIN
```
The IIS Admin Service was confirmed to be running.

![Screenshot: IIS service running after installation](assets/04_iis_installed.png)

### 6. Configuring IIS Web Server
After installing IIS, the lab demonstrated how to configure it to serve a custom website using the GUI management console.

**Steps:**
1. Opened **Internet Information Services (IIS) Manager** from the Start menu
2. Expanded the server node and navigated to **Sites**
3. Right-clicked and selected **Add Website**
4. Configured the new site:
   - **Site name:** Example
   - **Physical path:** `C:\Users\qwiklabs\amazingsite`
   - **Port:** 80

![Screenshot: IIS Manager showing the Add Website dialog](assets/05_add_website.png)

**Verification:**
The new website was successfully created and running on port 80. Accessing the server's external IP address in a browser displayed the custom site content.

![Screenshot: Custom website served by IIS](assets/06_website_running.png)

---

## GUI vs PowerShell Comparison
Throughout this lab, both GUI and command-line methods were demonstrated:

| Task | GUI Method | PowerShell Method | Best Use Case |
| :--- | :--- | :--- | :--- |
| View Service Status | Services console | `Get-Service` | GUI for single checks, PowerShell for scripting |
| Start/Stop Services | Right-click context menu | `Start-Service` / `Stop-Service` | PowerShell for automation |
| Change Startup Type | Service Properties dialog | `Set-Service -StartupType` | PowerShell for bulk changes |
| Install Features | Server Manager | `Install-WindowsFeature` | PowerShell for remote deployment |

![Screenshot: Services management console showing context menu options](assets/07_services_gui.png)

---

## Final Results and Conclusion
This lab successfully demonstrated the ability to manage Windows services through both graphical and command-line interfaces. Key accomplishments included:

✅ Listing and inspecting service details  
✅ Starting, stopping, and controlling service states  
✅ Enabling disabled services by modifying startup types  
✅ Installing Windows features via PowerShell  
✅ Configuring IIS to serve custom web content  

These skills are essential for any Windows system administrator, particularly for automating repetitive tasks and managing servers remotely. The combination of GUI knowledge for troubleshooting and PowerShell expertise for automation represents a well-rounded approach to Windows system administration.

![Screenshot: Final view of all services managed during the lab](assets/08_final_status.png)
