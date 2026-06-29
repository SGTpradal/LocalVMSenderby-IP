# Windows HTTP File Share GUI

A PowerShell tool with a graphical interface to temporarily share a local file over HTTP.

The goal is to make quick file transfers easier between a Windows computer and a destination machine, such as a Linux VM, local server, or host on the same network.

## Features

* Simple Windows Forms graphical interface.
* Shares a single file over HTTP.
* Automatically generates the `wget` command.
* Restricts access by destination IP address.
* Automatically creates a temporary Windows Firewall rule.
* Removes the firewall rule when sharing is stopped.
* Compatible with execution through a `.bat` file.
* Works regardless of the folder where the script is stored.

## Files

```text
Share-File-GUI.bat
Share-File-GUI.ps1
```

The `.bat` file is used only to start the PowerShell script with administrative permissions.

## Requirements

* Windows 10 or newer.
* PowerShell 5.1 or newer.
* Administrator permission.
* Destination machine reachable over the network.
* TCP port allowed locally by the script.

## How to Use

1. Download the project files.
2. Keep the `.bat` and `.ps1` files in the same folder.
3. Run the `.bat` file.
4. Accept the Windows administrator prompt.
5. Fill in the fields:

   * Windows/source IP address;
   * destination machine IP address;
   * file to be shared;
   * TCP port.
6. Click **Start sharing**.
7. Copy the generated `wget` command.
8. Run the command on the destination machine.

Example of a generated command:

```bash
wget "http://192.168.56.1:8000/file.zip"
```

## Usage Example

Scenario:

* Windows/source machine: `192.168.56.1`
* Linux/destination VM: `192.168.56.101`
* Port: `8000`
* File: `file.zip`

The script generates a temporary HTTP URL and allows the destination machine to download the file using `wget`.

## Security

This project was designed for local, controlled, and temporary use.

Important points:

* The script opens a temporary HTTP port on Windows.
* Access is limited to the specified destination IP address.
* There is no username/password authentication.
* Protection is based on IP restriction and a firewall rule.
* It is not recommended for use on public or unknown networks.
* Sharing should be stopped after the transfer is complete.

When clicking **Stop** or closing the window, the script removes the temporary Windows Firewall rule.

## Why Administrator Permission Is Required

The script needs to run as administrator because it creates and removes temporary Windows Firewall rules using `New-NetFirewallRule` and `Remove-NetFirewallRule`.

Without administrative permission, Windows does not allow firewall rules to be changed.

## About ExecutionPolicy Bypass

The `.bat` file may run PowerShell with:

```powershell
-ExecutionPolicy Bypass
```

This only affects the current PowerShell process. The system execution policy is not permanently changed.

This parameter is used to allow the script to run without requiring the user to manually change the Windows execution policy.

## Limitations

* Shares only one file at a time.
* Does not include authentication.
* Does not use HTTPS.
* Not intended to be exposed to the internet.
* Access depends on network connectivity between source and destination.
* External firewalls, NAT, or network rules may block the connection.

## Technical Structure

The script uses:

* `System.Windows.Forms` for the graphical interface.
* `System.Net.HttpListener` for the local HTTP server.
* `New-NetFirewallRule` to temporarily allow the port.
* `Start-Job` to keep the HTTP server running in the background.
* `wget` as the download command on the destination machine.

## Stopping the Share

The share can be stopped in two ways:

* by clicking the **Stop** button;
* by closing the application window.

In both cases, the script attempts to stop the background job and remove the temporary firewall rule.

## Notice

Use only in authorized environments.

This project is intended for administration, technical support, lab testing, and temporary file transfers between known machines.
