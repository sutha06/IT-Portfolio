# Lab 2.1: Managing System Services in Linux

**Course:** System Administration and IT Infrastructure Services (Course 4, Module 2)

---

## Lab Overview

The objective of this hands-on lab was to demonstrate essential **system administration skills** in a Linux environment. The core tasks involved controlling, troubleshooting, and configuring critical system services. This showcases a foundational ability to maintain and diagnose system health on a Linux server.

## Skills Demonstrated

* **Service Management:** Starting, stopping, restarting, and reloading services (`rsyslog`, `cups`).
* **Linux CLI:** Proficient use of commands like `sudo`, `mv`, `ls`, `tail`, `grep`, and `nano`.
* **Logging and Verification:** Using the `logger` command and verifying log output in `/var/log/syslog`.
* **Troubleshooting:** Diagnosing a failed service and restoring a missing configuration file.
* **Configuration Management:** Editing a service configuration file (`/etc/cups/cupsd.conf`) and applying changes using `reload` vs. `restart`.

---

## Step-by-Step Execution

### 1. Listing and Controlling the `rsyslog` Service

The process began with learning how to list and interpret the status of running services using the `service --status-all` command.

| Action | Command Used | Verification |
| :--- | :--- | :--- |
| **Check Status** | `service rsyslog status` | Verified service was **running**. |
| **Test Logging** | `logger "This is a test log entry"` then `tail /var/log/syslog` | Confirmed the log entry was successfully written. |
| **Stop Service** | `sudo service rsyslog stop` | Confirmed the service status was `not running... failed!` and that subsequent log attempts failed. |
| **Start Service** | `sudo service rsyslog start` | Verified the service was back to the `running` state. |

**Output of Running Services:**

(assets/01_rsyslog_check.png)

### 2. Troubleshooting and Fixing the `cups` Service

The `cups` (Common Unix Printing System) service was found to be in a **failed state**.

1.  **Diagnosis:** Checking the `/etc/cups` directory revealed that the main configuration file, `cupsd.conf`, was missing, but a backup, `cupsd.conf.old`, was available.
2.  **Repair:** The configuration file was restored using the move command:
    ```bash
    sudo mv /etc/cups/cupsd.conf.old /etc/cups/cupsd.conf
    ```
3.  **Start:** The service was then started: `sudo service cups start`.

The repair was successful, and the `cupsd` service status was verified to be running.


![Screenshot: Terminal showing the 'cupsd is running' output.](assets/02_cups_fix.png)

### 3. Configuring Services: Restart vs. Reload

This section demonstrated how to modify a service configuration and the difference between using `restart` and `reload`.

1.  **Configuration Change:** Used the `nano` editor to modify `/etc/cups/cupsd.conf`, changing `LogLevel warn` to `LogLevel debug`.
2.  **Restart:** Used `sudo service cups restart` to apply the change and verify debug messages were immediately written to `/var/log/cups/error_log`.
3.  **Reload:** The configuration was changed back from `debug` to `warn`. The change was then applied using `sudo service cups reload`. The `reload` action was confirmed to apply the change **without stopping and starting** the underlying process, which is critical for services that must maintain continuous uptime.

---

## Final Results and Conclusion

This lab successfully demonstrated the ability to monitor, control, troubleshoot, and reconfigure system services in a Linux environment. These are non-negotiable, day-to-day skills for a System Administrator.




![Screenshot: Final status check proving the cups service remained running after the reload action.](assets/03_final_reload.png)

