To install MySQL on your VPS (assuming it's running a Linux distribution like Ubuntu or CentOS), you can follow these steps:

### 1. Update your package list:
First, make sure your package list is up to date.

```bash
sudo apt update
```

### 2. Install MySQL Server:
For **Ubuntu** or **Debian-based** systems, use the following command:

```bash
sudo apt install mysql-server
```

For **CentOS** or **RHEL-based** systems, use this:

```bash
sudo yum install mysql-server
```

### 3. Secure MySQL Installation:
Once MySQL is installed, run the security script to set up a root password, remove insecure default settings, and improve the overall security of your MySQL installation.

```bash
sudo mysql_secure_installation
```

Follow the prompts to:

- Set a root password (if you haven't already).
- Remove the test database and access to it.
- Reload privilege tables to apply changes.

### 4. Start MySQL Service:
Start the MySQL service using the following command:

```bash
sudo systemctl start mysql
```

To ensure that MySQL starts automatically on boot:

```bash
sudo systemctl enable mysql
```

### 5. Verify MySQL Installation:
You can verify that MySQL is running with this command:

```bash
sudo systemctl status mysql
```

Or by logging into the MySQL shell:

```bash
sudo mysql -u root -p
```

### 6. Allow Remote Connections (optional):
If you want to allow remote connections to your MySQL server, you'll need to:

- Edit the MySQL configuration file `/etc/mysql/mysql.conf.d/mysqld.cnf` (for Ubuntu/Debian) or `/etc/my.cnf` (for CentOS/RHEL).
- Change the `bind-address` from `127.0.0.1` to your server's public IP address or `0.0.0.0` to allow all IPs (use with caution).

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Look for the `bind-address` directive and update it:

```
bind-address = 0.0.0.0
```

- Restart MySQL after making the changes:

```bash
sudo systemctl restart mysql
```

### 7. Configure Firewall (optional):
If you're using a firewall, you might need to open the MySQL port (default is `3306`).

For **UFW** firewall (on Ubuntu/Debian):

```bash
sudo ufw allow 3306
```

For **Firewalld** (on CentOS/RHEL):

```bash
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
sudo firewall-cmd --reload
```

### 8. Create a MySQL User (optional):
To create a new MySQL user with specific privileges, log into MySQL and run:(must login as a root user)

```bash
CREATE USER 'newuser'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

This will allow the `newuser` to connect from any IP address (`%`) with full privileges.

Let me know if you need more details based on your VPS setup!
