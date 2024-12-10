# New User-Group Setup

## Linux
- Enable WSL Feature
First, open PowerShell as Administrator and run
```shell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
- Restart your to complete the feature installation.
- Download and install Ubuntu from Microsoft store.
- Set WSL 2 as Default, open PowerShell and run:
```shell
wsl --set-default-version 2
```
- In PowerShell, install the latest Ubuntu LTS version:
```shell
wsl --install -d Ubuntu
```

This command will:

- Download Ubuntu
- Install WSL
- Set up a new Ubuntu instance

When prompted, create a username and password for your Ubuntu installation.

Verify Installation. Check WSL and Ubuntu status:
```shell
wsl --list --verbose
```
