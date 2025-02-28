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



# PostgreSQL Database Setup Guide

## Install PostgreSQL
  
On Ubuntu/Debian, update your package list and install PostgreSQL and its contrib package:
```bash
sudo apt update && sudo apt install postgresql postgresql-contrib
```

Note: On macOS you might use Homebrew. In that case, the default superuser may be your macOS username rather than "postgres".

## Start and Enable the PostgreSQL Service

Make sure PostgreSQL is running and enabled at startup:
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

## Connect to PostgreSQL as the Superuser

By default, PostgreSQL creates a superuser named postgres. Connect using:
```bash
sudo -u postgres psql
```
⚠️ Pitfall: If you see this error:
```bash
psql: error: connection to server on socket "/tmp/.s.PGSQL.5432" failed: FATAL: role "postgres" does not exist
```
This typically means your installation (especially on macOS with Homebrew) uses a different default role. Try connecting with your system username:
```bash
psql -U your_username -d postgres
```

## Create a New User/Role and Database

Inside the psql shell, run these commands:
```sql
-- Create a new user with a password
CREATE USER your_username WITH PASSWORD 'your_password';

-- Create a new database
CREATE DATABASE your_database_name;

-- Grant all privileges on the database to your user
GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_username;
```

## Grant Additional Permissions on the Public Schema

These commands ensure your user can create and manage objects in the public schema:
```sql
GRANT CREATE ON SCHEMA public TO your_username;
GRANT ALL PRIVILEGES ON SCHEMA public TO your_username;
ALTER USER your_username WITH CREATEDB;
```

## Set the Database Owner (Optional)
If you want your new database to be owned by your user:
```sql
ALTER DATABASE your_database_name OWNER TO your_username;
```

## Exit the psql Shell
Type either:
```
\q
```
or simply:
```
exit
```

## Connect to Your New Database

You can now connect using either of these methods:
- Using a Connection URL in your application:
```bash
postgresql://your_username:your_password@localhost:5432/your_database_name
```
- Using psql from the command line:
```bash
psql -U your_username -d your_database_name
```

## Common Pitfalls and Solutions

- Missing Database Error: If you run psql without specifying a database, it defaults to one matching your system username. Either create that database or specify a different one with `-d`.
- Role "postgres" Not Found: On some installations (like Homebrew on macOS), the default superuser isn't "postgres" but your macOS username. Connect using your username instead.
- Locale Issues During Initialization: If reinitializing your database cluster, ensure your environment variables are set correctly:
```bash
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```
- Permission Issues with initdb: Make sure the target directory is writable by your user. For macOS users who need to reinitialize:
```bash
sudo mkdir -p /usr/local/var
sudo chown -R $(whoami) /usr/local/var
initdb /usr/local/var/postgres -E utf8
```
⚠️ Never run initdb as root; PostgreSQL requires the cluster to be owned by an unprivileged user.

### Extra
- You can recreate the databases using the `createdb` command. For example:
```bash
createdb postgres
createdb your_username
```
- For the opposite, use the `dropdb` command.
