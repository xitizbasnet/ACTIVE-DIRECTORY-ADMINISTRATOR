# 📘 Appendix: Common Errors & Best Practices

## 📋 Overview

This appendix provides troubleshooting guidance, PowerShell best practices, and frequently used Active Directory administration one-liners for IT administrators.

---

# 🚨 Common Active Directory Errors & Troubleshooting

## ❌ Error 1: "Cannot Contact a Domain Controller"

### 🔍 Problem

The workstation or server cannot communicate with an available Domain Controller.

### ✅ Troubleshooting Steps

#### 1. Check Network Connectivity

```powershell
ping domaincontroller
```

Verify that the Domain Controller responds over the network.

---

#### 2. Verify DNS Resolution

```powershell
nslookup yourdomain.com
```

Confirm that DNS can correctly resolve the domain name.

---

#### 3. Test Kerberos Authentication

```powershell
klist
```

Review Kerberos tickets and authentication status.

---

#### 4. Force Group Policy Update

```powershell
gpupdate /force
```

Refresh applied Group Policy settings.

---

# ❌ Error 2: "User Not Found" in Get-ADUser

## 🔍 Problem

The specified Active Directory user account cannot be located.

---

## ✅ Troubleshooting Steps

### 1. Verify User Exists

```powershell
Get-ADUser -Filter { SamAccountName -eq 'username' }
```

Search Active Directory for the specified username.

---

### 2. Check Organizational Unit Path

```powershell
Get-ADUser -SearchBase 'OU=Users,DC=yourdomain,DC=com'
```

Confirm that the user exists within the expected OU.

---

### 3. Specify Domain Controller Server

```powershell
Get-ADUser -Identity user -Server DC1.yourdomain.com
```

Query a specific Domain Controller.

---

# ❌ Error 3: "Permission Denied" During Operations

## 🔍 Problem

The executing account does not have sufficient permissions to perform the requested operation.

---

## ✅ Troubleshooting Steps

### 1. Run PowerShell with Elevated Privileges

```
Right-click PowerShell → Run as Administrator
```

Run PowerShell with appropriate administrative rights.

---

### 2. Check Explicit Permissions

Review:

* Active Directory delegation
* OU permissions
* Group membership
* Object-level access control

---

### 3. Test with Alternate Credentials

```powershell
Get-ADUser -Identity user -Credential $Cred
```

Run the query using alternate credentials.

---

# ⭐ PowerShell Best Practices

## 🔐 Security & Administration Guidelines

| Recommendation                                               | Purpose                              |
| ------------------------------------------------------------ | ------------------------------------ |
| ✅ Always use `-ErrorAction Stop` in try-catch blocks         | Ensures errors are properly captured |
| 📝 Log all important operations to files                     | Provides audit history               |
| 🔍 Use `-WhatIf` to preview changes before executing         | Prevents accidental modifications    |
| ⚠️ Implement confirmation prompts for destructive operations | Adds safety controls                 |
| 🔑 Use `SecureString` for passwords, never plain text        | Protects credentials                 |
| 🧪 Test scripts in a lab environment first                   | Reduces production risk              |
| 💾 Schedule backups before running bulk changes              | Enables recovery                     |
| 📚 Document all custom scripts with inline comments          | Improves maintainability             |

---

# 🛠️ Useful Active Directory PowerShell One-Liners

## 👤 List Inactive Users

```powershell
Get-ADUser -Filter { LastLogonDate -lt (Get-Date).AddMonths(-3) }
```

**Purpose:** Finds users who have not logged in for more than three months.

---

## 🔢 Count All Users

```powershell
(Get-ADUser -Filter *).Count
```

**Purpose:** Returns the total number of user accounts in Active Directory.

---

## 👥 Find User's Group Memberships

```powershell
Get-ADGroupMember -Identity 'GroupName' |
Where Name -like '*username*'
```

**Purpose:** Searches for a user within a specific group.

---

## 🔐 Reset All Passwords in an OU

```powershell
Get-ADUser -Filter * -SearchBase 'OU=X,DC=y' |
Set-ADAccountPassword -Reset `
-NewPassword (
    ConvertTo-SecureString `
    -String 'NewPass' `
    -AsPlainText `
    -Force
)
```

**Purpose:** Resets passwords for all users within a specified Organizational Unit.

⚠️ **Warning:** Test in a non-production environment before execution.

---

## 📤 Export All Users to CSV

```powershell
Get-ADUser -Filter * |
Select Name,SamAccountName,Mail |
Export-Csv users.csv
```

**Purpose:** Exports Active Directory users and email information into a CSV file.

---

## 🔎 Check Last Password Change

```powershell
Get-ADUser -Identity username -Properties PasswordLastSet
```

**Purpose:** Displays the last password modification time for a user account.

---

# 📚 Administrator Quick Reference

## Common Troubleshooting Flow

```
Connectivity Issue
        |
        ↓
Check Network
        |
        ↓
Validate DNS
        |
        ↓
Verify Domain Controller
        |
        ↓
Check Authentication
        |
        ↓
Review Permissions
```

---

# ✅ Documentation Summary

This appendix covers:

* 🚨 Active Directory troubleshooting
* 🌐 Domain Controller connectivity checks
* 🔐 Authentication validation
* 👤 User lookup issues
* 🛡️ Permission troubleshooting
* ⭐ PowerShell administration standards
* 🛠️ Daily-use AD commands

---

> 💡 **Best Practice Reminder**
>
> Always validate scripts in a test environment before executing against production Active Directory systems. Maintain current backups and document all administrative changes.
