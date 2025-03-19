# Rsyslog with Web GUI - Automated Installation

## Overview
This script automates the installation and configuration of Rsyslog with a Web GUI for centralized log management. It installs MySQL, sets up a database for log storage, configures Rsyslog to log messages to both files and MySQL, and deploys a web-based UI for viewing logs.

## Features
- **Automated Setup**: Installs and configures MySQL, Apache, PHP, Rsyslog, and the web UI.
- **Database Integration**: Stores system logs in a MySQL database for structured log analysis.
- **Web-based UI**: Provides an easy-to-use interface for viewing and managing logs.
- **Support for Multiple Configurations**: Automatically detects and applies the correct Rsyslog-to-MySQL configuration.
- **Log File Storage**: Creates log files under `/var/log/` for redundancy.
- **Automated Maintenance**: Sets up a cron job for periodic database maintenance.

## Prerequisites
- A Debian-based Linux system (Ubuntu, Debian, etc.)
- Root privileges (sudo access)
- Internet connection for package installation

## Installation
### Step 1: Download the Script
```bash
wget https://your-repository.com/ultimate.sh -O ultimate.sh
```
### Step 2: Grant Execute Permissions
```bash
chmod +x ultimate.sh
```
### Step 3: Run the Script as Root
```bash
sudo ./ultimate.sh
```
Follow the on-screen instructions. The script will install dependencies, configure services, and deploy the web UI.

## Default Configuration
### MySQL Database
- **Database Name**: `syslog_db`
- **Table Name**: `SystemEvents`
- **Username**: `root`
- **Password**: `Admin@_1234`

### Rsyslog Logging
- Logs stored in `/var/log/`
- Logs inserted into MySQL for easy querying
- Accepts logs via **UDP (port 514)** and **TCP (port 514)**

### Web UI Access
- **URL**: `http://localhost/syslog-ui`
- **Config File**: `/var/www/html/syslog-ui/config.php`

## Post-Installation Verification
To ensure everything is working correctly, run the following checks:

### Check Rsyslog Service Status
```bash
systemctl status rsyslog
```
### Check if Logs are Being Inserted into MySQL
```bash
mysql -u root -p -e "USE syslog_db; SELECT * FROM SystemEvents LIMIT 10;"
```
### Manually Send a Test Log
```bash
logger -t "TestScript" "This is a test log message"
```
### Verify Log Entry in MySQL
```bash
mysql -u root -p -e "USE syslog_db; SELECT * FROM SystemEvents ORDER BY ID DESC LIMIT 5;"
```

## Troubleshooting
If you encounter issues, check the following logs:

### Rsyslog Logs
```bash
journalctl -u rsyslog -n 50
```
### MySQL Error Logs
```bash
tail -n 50 /var/log/mysql/error.log
```
### Apache Logs
```bash
tail -n 50 /var/log/apache2/error.log
```

## Uninstallation
To remove all installed components, run:
```bash
sudo apt remove --purge mysql-server apache2 php libapache2-mod-php rsyslog rsyslog-mysql git php-mysql php-xml -y
sudo rm -rf /var/log/$(hostname) /var/www/html/syslog-ui
sudo rm -rf /etc/rsyslog.d/50-mysql.conf /etc/rsyslog.conf
```

## License
This script is open-source and can be freely modified or distributed.

## Author
**Akshat Singh**

