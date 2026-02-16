# 🖥️ GetRDP-Free

<div align="center">

![Windows](https://img.shields.io/badge/Windows-Server-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![Ngrok](https://img.shields.io/badge/Ngrok-1F1E37?style=for-the-badge&logo=ngrok&logoColor=white)
![Free](https://img.shields.io/badge/100%25-FREE-00D26A?style=for-the-badge)

### Get Free Windows Server & RDP Access Using GitHub Actions & Ngrok Tunnel

*No credit card required • 100% Free • Always Available*

[📖 Documentation](#-how-it-works) • [🚀 Quick Start](#-quick-start-guide) • [⚡ Features](#-features) • [❓ FAQ](#-faq)

</div>

---

## 🎯 What is GetRDP-Free?

**GetRDP-Free** is a powerful method to get **FREE Windows Server RDP access** using GitHub Actions and Ngrok tunneling. Perfect for testing, development, or running Windows-based applications without paying for cloud services!

### ✨ Features

- 🆓 **100% Free** - No credit card required
- ⚡ **Quick Setup** - Get started in under 5 minutes
- 🔒 **Private & Secure** - Uses your own GitHub account
- 🔄 **Unlimited Usage** - Create new sessions anytime
- 💪 **Full RDP Access** - Complete Windows Server control
- 🌐 **Global Access** - Connect from anywhere

---

## 🚀 Quick Start Guide

### Step 1️⃣: Setup GitHub Account

1. **Create a GitHub Account** (if you don't have one)
   - Visit: [https://github.com/signup](https://github.com/signup)
   - Sign up with your email

2. **Create a Private Repository**
   ```
   Repository Name: GetRDP-Free (or any name you prefer)
   Visibility: Private ✅
   ```

3. **Configure Repository Secrets**
   - Navigate to: `Settings` → `Secrets and variables` → `Actions`
   - Click `New repository secret`

---

### Step 2️⃣: Setup Ngrok

1. **Create Ngrok Account**
   - Visit: [https://ngrok.com/signup](https://ngrok.com/signup)
   - Sign up for free

2. **Get Your Auth Token**
   - After login, go to: [https://dashboard.ngrok.com/get-started/your-authtoken](https://dashboard.ngrok.com/get-started/your-authtoken)
   - Copy your authtoken

3. **Add to GitHub Secrets**
   - In your GitHub repository secrets, create:
   ```
   Name: NGROK_AUTH_TOKEN
   Value: [Paste your ngrok authtoken here]
   ```
   - Click `Add secret`

---

### Step 3️⃣: Create GitHub Workflow

1. **Create Workflow File**
   - In your repository, go to `Actions` tab
   - Click `New workflow` → `set up a workflow yourself`
   - Name it: `main.yml`

2. **Paste This Code**

```yaml
name: CI
on: [push, workflow_dispatch]

jobs:
  build:
    runs-on: windows-latest
    
    steps:
    - name: Download Ngrok
      run: Invoke-WebRequest https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-windows-amd64.zip -OutFile ngrok.zip
    
    - name: Extract Ngrok
      run: Expand-Archive ngrok.zip
    
    - name: Authenticate Ngrok
      run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
      env:
        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
    
    - name: Enable Remote Desktop
      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
    
    - name: Enable Firewall Rule
      run: Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
    
    - name: Set Authentication
      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1
    
    - name: Set User Password
      run: Set-LocalUser -Name "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "P@ssw0rd!" -Force)
    
    - name: Create Ngrok Tunnel
      run: .\ngrok\ngrok.exe tcp 3389
```

3. **Commit the workflow**
   - Click `Commit changes`

---

### Step 4️⃣: Start Your RDP Session

1. **Run the Workflow**
   - Go to `Actions` tab
   - Click on the workflow
   - Click `Run workflow` → `Run workflow`

2. **Get Connection Details**
   - Wait for the workflow to run (~2-3 minutes)
   - Click on the running job → `Create Ngrok Tunnel` step
   - Look for the ngrok URL (format: `tcp://X.tcp.ngrok.io:XXXXX`)

3. **Connect via RDP**

   **Windows Users:**
   - Open `Remote Desktop Connection` (mstsc)
   - Enter the ngrok address: `X.tcp.ngrok.io:XXXXX`
   - Click `Connect`

   **Mac Users:**
   - Download Microsoft Remote Desktop from App Store
   - Add PC with ngrok address
   - Connect

   **Linux Users:**
   ```bash
   remmina
   # or
   xfreerdp /v:X.tcp.ngrok.io:XXXXX /u:runneradmin
   ```

4. **Login Credentials**
   ```
   Username: runneradmin
   Password: P@ssw0rd!
   ```

---

## ♾️ How to Use for Free Lifetime (Always Available Method)

### 🔄 Method 1: Manual Trigger (Recommended)

**Best for:** On-demand usage when you need RDP access

1. Whenever you need RDP access, go to your repository
2. Navigate to `Actions` tab
3. Select your workflow
4. Click `Run workflow`
5. Get the new ngrok URL and connect

**💡 Tip:** Each session lasts up to 6 hours (GitHub Actions limit)

---

### 🔄 Method 2: Auto-Trigger on Push

**Best for:** Frequent users who want automatic sessions

Update your workflow trigger:

```yaml
name: CI
on: 
  push:
  workflow_dispatch:
  schedule:
    - cron: '0 */6 * * *'  # Runs every 6 hours
```

**⚠️ Note:** Be mindful of GitHub Actions usage limits

---

### 🔄 Method 3: Multiple Repositories

**Best for:** Maximum uptime and redundancy

1. Create multiple private repositories (e.g., `GetRDP-1`, `GetRDP-2`, `GetRDP-3`)
2. Set up the same workflow in each
3. Rotate between repositories when one session expires
4. This gives you near-continuous access

---

### 🔄 Method 4: Extended Sessions

**For longer sessions:**

Add a keep-alive step to your workflow:

```yaml
    - name: Keep Alive
      run: |
        while ($true) {
          Start-Sleep -Seconds 300
          Write-Host "Session active: $(Get-Date)"
        }
```

---

## 🛠️ How It Works

```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│   GitHub    │────────▶│   Windows    │◀────────│   Ngrok     │
│   Actions   │         │   Server     │         │   Tunnel    │
└─────────────┘         └──────────────┘         └─────────────┘
                                │                        ▲
                                │                        │
                                └────────────────────────┘
                                    Your RDP Connection
```

1. **GitHub Actions** provisions a free Windows Server runner
2. **Ngrok** creates a secure tunnel to the server's RDP port (3389)
3. **You** connect through the ngrok URL with RDP client
4. **Enjoy** full Windows Server access for free!

---

## ⚙️ Customization Options

### Change Password

Edit the workflow file:

```yaml
- name: Set User Password
  run: Set-LocalUser -Name "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "YourNewPassword123!" -Force)
```

### Add Additional Users

```yaml
- name: Create New User
  run: |
    New-LocalUser -Name "newuser" -Password (ConvertTo-SecureString -AsPlainText "NewP@ssw0rd!" -Force)
    Add-LocalGroupMember -Group "Administrators" -Member "newuser"
```

### Install Software Automatically

```yaml
- name: Install Chrome
  run: |
    Invoke-WebRequest -Uri "https://dl.google.com/chrome/install/latest/chrome_installer.exe" -OutFile "chrome.exe"
    Start-Process -FilePath "chrome.exe" -Args "/silent /install" -Wait
```

---

## 📋 System Specifications

| Component | Specification |
|-----------|---------------|
| **OS** | Windows Server 2022 |
| **CPU** | 2-core CPU |
| **RAM** | 7 GB |
| **Storage** | 14 GB SSD |
| **Network** | Fast Internet |
| **Duration** | Up to 6 hours per session |

---

## ❓ FAQ

<details>
<summary><b>Is this really free?</b></summary>

Yes! GitHub Actions provides free minutes for public and private repositories. Ngrok's free tier is sufficient for RDP tunneling.

</details>

<details>
<summary><b>How long does each session last?</b></summary>

Each GitHub Actions job can run for up to 6 hours. After that, you'll need to start a new session.

</details>

<details>
<summary><b>Can I use this for production?</b></summary>

This is intended for development, testing, and educational purposes. For production workloads, consider dedicated cloud services.

</details>

<details>
<summary><b>What if the session disconnects?</b></summary>

Simply restart the workflow from the Actions tab to get a new ngrok URL and reconnect.

</details>

<details>
<summary><b>Can I access my files after reconnecting?</b></summary>

No, each session creates a fresh Windows Server instance. Save important files to GitHub, cloud storage, or download them before the session ends.

</details>

<details>
<summary><b>Is my data secure?</b></summary>

The connection is tunneled through Ngrok with encryption. However, avoid storing sensitive data as GitHub Actions runners are temporary.

</details>

---

## ⚠️ Important Notes

- 📝 **Save Your Work:** The server is temporary - save important files externally
- 🔒 **Change Default Password:** For better security, modify the default password
- ⏱️ **Time Limit:** GitHub Actions jobs are limited to 6 hours
- 💾 **No Persistence:** Files are deleted when the session ends
- 📊 **Usage Limits:** Free GitHub accounts have monthly Action minutes limits

---

## 🎯 Use Cases

- ✅ Testing Windows applications
- ✅ Running Windows-specific software
- ✅ Development and debugging
- ✅ Learning Windows Server administration
- ✅ Running automated tasks
- ✅ Browser automation testing
- ✅ Temporary Windows environment

---

## 🤝 Contributing

Contributions are welcome! Feel free to:

- 🐛 Report bugs
- 💡 Suggest features
- 📝 Improve documentation
- 🔧 Submit pull requests

---

## ⚖️ Disclaimer

This project is for educational and development purposes. Please comply with:
- GitHub's [Terms of Service](https://docs.github.com/en/github/site-policy/github-terms-of-service)
- Ngrok's [Terms of Service](https://ngrok.com/tos)
- Use responsibly and ethically

---

## 📞 Support

Having issues? 

1. Check the [FAQ](#-faq) section
2. Review GitHub Actions logs for errors
3. Verify your ngrok token is correct
4. Open an issue in this repository

---


## ⭐ Star This Repository

If you found this helpful, please give it a star! It helps others discover this project.

---

<div align="center">

### Tpis by me :b 

**Happy RDP-ing! 🚀**

</div>
