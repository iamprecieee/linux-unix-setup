# New User-Group Setup

## Linux
- Install WSL
```shell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux









apt-get update && apt-get install -y \
    git \
    curl \
    build-essential \
    wget && \
    rm -rf /var/lib/apt/lists/* && \
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y







- Create a new group:
```bash
sudo groupadd <new_group_name>
```

- Create a new user:
```bash
sudo useradd -r -g  <new_username>
```
The -m flag creates a home directory for the user.

- Set a password for the new user:
```bash
sudo passwd <new_username>
```
You'll be prompted to enter and confirm the new password.

- Add the user to the new group:
```bash
sudo usermod -aG <new_group_name> <new_username>
```

- Verify the user and group creation:
```bash
id <new_username>
```

- To switch user:
```bash
su <new_username>
```


## Mac0S
- Create a new group:
```bash
sudo dscl . -create /Groups/<new_group_name>
```
- Create a group ID for the new group:
```bash
sudo dscl . -create /Groups/anon PrimaryGroupID <new_group_ID>
```
Replace "new_group_ID" with any unused group ID number. To find an unused group ID, you can check the last used ID with:
```bash
dscl . -list /Groups PrimaryGroupID | sort -n -k2 | tail -n 1
```

- Create a new user:
```bash
sudo dscl . -create /Users/<new_username>
sudo dscl . -create /Users/<new_username> UserShell /bin/bash
sudo dscl . -create /Users/<new_username> RealName "New User"
sudo dscl . -create /Users/<new_username> UniqueID <new_unique_ID>
sudo dscl . -create /Users/<new_username> PrimaryGroupID <new_primary_group_ID>
sudo dscl . -create /Users/<new_username> NFSHomeDirectory /Users/<new_username>
```
Replace "New User" with the full name, "new_unique_ID" and "new_primary_group_ID" with unused IDs.

- Set a password for the new user:
```bash
sudo passwd <new_username>
```

- Add the user to the new group:
```bash
sudo dscl . -append /Groups/<new_group_name> GroupMembership <new_username>
```

- Verify the user and group creation:
```bash
dscl . -read /Users/<new_username>
dscl . -read /Groups/<new_group_name>
```

- To switch user:
```bash
su - <new_username>
```

- Set up keychain:
```bash
security create-keychain ~/Library/Keychains/login.keychain-db
security default-keychain -s ~/Library/Keychains/login.keychain-db
```
