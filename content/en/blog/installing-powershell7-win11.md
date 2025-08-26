---
title: "Installing PowerShell 7 on Windows 11"
titleLink: "Installing PowerShell 7 on Windows 11: The Recommended Methods for IT Pros"
date: 2023-10-27
description: >
  A comprehensive guide for system administrators and IT professionals on the best practices for installing PowerShell 7 on Windows 11, covering Winget, MSI, and Microsoft Store methods.
---

In the world of Windows system administration, PowerShell is the cornerstone of automation and configuration management. While Windows 11 ships with the reliable Windows PowerShell 5.1 (`powershell.exe`), the modern, cross-platform **PowerShell 7** (`pwsh.exe`) is where the future lies. It's built on .NET, offers superior performance, and receives all the latest features.

This guide provides a clear, operations-focused walkthrough of the recommended methods for installing PowerShell 7, ensuring you can leverage its full potential in your daily tasks.

## Understanding the Coexistence

First, it's crucial to know that installing PowerShell 7 **does not** replace Windows PowerShell 5.1. They are installed side-by-side and can be used independently. This is by design, ensuring backward compatibility for legacy scripts while allowing you to use the modern shell for new development and administration.

*   **Windows PowerShell 5.1:** `powershell.exe`
*   **PowerShell 7:** `pwsh.exe`

Now, let's dive into the installation methods, ordered by recommendation for professional environments.

### Method 1: Winget (The Modern & Automated Approach)

The Windows Package Manager (`winget`) is the de-facto command-line tool for software management on Windows 11. For any IT professional, this is the most efficient and scriptable method.

**Advantages:**
*   **Fast and Scriptable:** Ideal for automated deployments and new machine setups.
*   **Easy Updates:** A single command keeps PowerShell up-to-date.
*   **Official Source:** The package is maintained directly by Microsoft.

**Steps:**

1.  Open **Windows Terminal** or **PowerShell** as an **Administrator**.
2.  Install the latest stable version of PowerShell with a single command:
    ```powershell
    winget install --id Microsoft.PowerShell --source winget
    ```
3.  To upgrade an existing installation in the future, simply run:
    ```powershell
    winget upgrade Microsoft.PowerShell
    ```

### Method 2: MSI Installer (For Traditional & Flexible Control)

Using the official MSI package from GitHub gives you granular control over the installation process. This method is perfect for manual installations or for deployment via enterprise tools like MECM (SCCM) or Microsoft Intune.

**Advantages:**
*   **Full Customization:** Control over context menus, path variables, and PowerShell Remoting setup.
*   **Offline Installation:** The MSI can be downloaded and distributed on isolated networks.
*   **Enterprise Deployment:** The standard for distributing software via management systems.

**Steps:**

1.  Navigate to the official [PowerShell GitHub Releases page](https://github.com/PowerShell/PowerShell/releases).
2.  Find the latest release (marked "Latest").
3.  Under the **Assets** section, download the `PowerShell-7.x.x-win-x64.msi` file.
4.  Run the installer and configure the options in the wizard to suit your needs. We highly recommend keeping **"Add PowerShell to Path Environment Variable"** enabled.

### Method 3: Microsoft Store (The Simplest for Individual Use)

For maximum simplicity and automatic updates, the Microsoft Store is an excellent choice. It's perfect for individual users or development machines where hands-off maintenance is a priority.

**Advantages:**
*   **One-Click Install:** Effortless setup.
*   **Automatic Updates:** The Microsoft Store handles updates in the background.

**Steps:**

1.  Open the **Microsoft Store** app.
2.  Search for **"PowerShell"**.
3.  Select the application and click **"Install"**.

## Post-Installation Verification

Regardless of the method you chose, verify the installation was successful.

1.  Open a new **Windows Terminal** tab (it should automatically detect PowerShell 7) or search for "PowerShell 7" in the Start Menu.
2.  In the new terminal, you should be running `pwsh.exe`.
3.  Confirm the version by checking the `$PSVersionTable` variable:

    ```powershell
    $PSVersionTable
    ```

The output will display the `PSVersion`, confirming you are running 7.x.x. You are now ready to harness the power of modern PowerShell!
