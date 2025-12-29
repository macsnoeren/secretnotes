# Local Password Manager Manual

## Contents
1. [Introduction](#introduction)
2. [Installation & Getting Started](#installation--getting-started)
3. [Vaults](#vaults)
4. [Tabs and Items](#tabs-and-items)
5. [Passwords and Notes](#passwords-and-notes)
6. [Dark Mode](#dark-mode)
7. [Auto-Lock](#auto-lock)
8. [Copying and Clipboard Auto-Wipe](#copying-and-clipboard-auto-wipe)
9. [Vault Backup & Restore](#vault-backup--restore)
10. [Deleting a Vault](#deleting-a-vault)
11. [Security Features](#security-features)
12. [Tips](#tips)

---

## Introduction
The **Local Password Manager** is a fully client-side password manager.  
All data is stored locally in your browser and encrypted using a master password.

---

## Installation & Getting Started
1. Open `password-manager.html` in a modern browser (Chrome, Firefox, Edge).  
2. Enter a **master password** to create a new vault or open an existing one.

---

## Vaults
- A vault is a secure storage for all your passwords and notes.  
- Every vault is encrypted with your master password (AES via SJCL).  
- If a vault already exists and you create a new one, you will be prompted to confirm overwriting it.

---

## Tabs and Items
- Tabs help organize your passwords/notes (e.g., Work, Personal).  
- Items are individual passwords or secure notes.  

**Actions:**
- New tab: click `+` next to the tab bar.  
- New item: click `+` in the Items section → choose `Password` or `Secure Note`.  
- Select an item to view or edit its details.

---

## Passwords and Notes
**Password item:**
- Fields: Title, Username, Password, URL, Notes.  
- Password fields are hidden by default and with the (`👁️`) the password is made visible.  
- Buttons for generating (`🎲`) and copying (`📋`) passwords to the clipboard.  

**Secure Note:**
- Fields: Title and Content.  
- For free text, secrets, or other sensitive information.

---

## Dark Mode
- Toggle dark mode with the `Toggle dark mode` button.  
- Dark mode changes background, text, input fields, tabs, alerts, and navbar to darker colors for better privacy.

---

## Auto-Lock
- The vault automatically locks after **X minutes** of inactivity (`AUTO_LOCK_MINUTES`).  
- Mouse movement, scrolling, and keyboard input reset the timer.  
- On auto-lock, the master password and all sensitive fields are cleared.

---

## Copying and Clipboard Auto-Wipe
- When a password is copied:
  - It is placed on the clipboard.  
  - After **30 seconds**, it is automatically cleared.  
- Closing the vault or refreshing the browser also clears the clipboard (best effort).

---

## Vault Backup & Restore
- **Backup:** download your vault as an **encrypted file**.  
- **Restore:** upload an existing encrypted vault file.  
- If a vault already exists, a confirmation is required to overwrite it.

---

## Deleting a Vault
- Click `Delete Vault` or delete individual items/tabs.  
- A confirmation prompt appears to prevent accidental deletion.

---

## Security Features
1. **Encryption:** AES via SJCL using the master password.  
2. **Best-Effort Memory Wipe:** Vault objects, passwords, and clipboard content are overwritten.  
3. **Clipboard Auto-Wipe:** Sensitive information is automatically cleared.  
4. **Auto-Lock:** Vault locks itself after inactivity.  
5. **Integrity Check:** Optional verification of external resources via SRI.  
6. **Dark Mode:** Enhances privacy visually.

---

## Tips
- Use a strong master password.  
- Regularly backup your vault.  
- Lock the vault or close the browser when leaving your computer.  
- Check alert messages at the bottom of the screen for important notifications.  

---

**End of the manual**