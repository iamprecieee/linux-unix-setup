# New User-Group Setup

## Linux
- Enable WSL Feature
First, open PowerShell as Administrator and run
```shell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
- Restart your computer to complete the feature installation.
- Download and install Ubuntu 22.04 LTS from Microsoft store.
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

To launch Ubuntu from the terminal:
```shell
wsl
```

To setup a new user:
```bash
adduser <new_username>
# Then set and confirm password
```
- Add new user to sudoers group to grant `sudo` privileges.
```bash
usermod -aG sudo <new_username>
```
- Switch to new user
```bash
su <new_username>
```

### Extras
To install necessary dependencies for python on WSL:
```bash
sudo apt update && sudo apt upgrade && apt install -y \
    git \
    curl \
    build-essential \
    pkg-config \
    libssl-dev \
    wget \
    protobuf-compiler \
    python3 \
    python3-pip \
    unzip \
    python3-venv
```


# Install and setup postgresql

- Install postgres:
```bash
sudo apt update && sudo apt install postgresql postgresql-contrib
```

- Start and enable the postgres service:
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

- Connect via default user:
```bash
sudo -u postgres psql
```

- Create new user/role:
```sql
CREATE USER <your_username> WITH PASSWORD '<your_password>';
CREATE DATABASE <your_database_name>;
GRANT ALL PRIVILEGES ON DATABASE <your_database_name> TO <your_username>;
```

- Grant permissions to new user:
```sql
GRANT CREATE ON SCHEMA public TO <your_username>;
GRANT ALL PRIVILEGES ON SCHEMA public TO <your_username>;
ALTER USER <your_username> WITH CREATEDB;
```

- Set database ownership to new user:
```sql
ALTER DATABASE <your_database_name> TO <your_username>;
```

- To exit psql:
```bash
exit
```

- Connect to your db using URL:
```bash
postgresql://<your_username>:<your_password>@localhost:5432/<your_database_name>
```

- Connect to your db using psql:
```bash
psql -U <your_username> -d <your_database_name>
```