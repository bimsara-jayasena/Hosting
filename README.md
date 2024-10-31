# Hosting
## 1. Choose a VPS Plan
Purchase a VPS plan on Hostinger or another provider.
Make sure it meets your app's resource requirements, such as storage, memory, and bandwidth.

## 2. Access the VPS
Once your VPS is ready, log in to the server using SSH. You’ll need the IP address, username, and password (or private key).
On Windows, you can use an SSH client like PuTTY, and on macOS/Linux, you can use the terminal:

``` bash
  ssh username@your_vps_ip
```

## 5. Install PHP
Install PHP and necessary modules. The modules required depend on your app’s functionality.
```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

## 6. Configure the Web Server

Place your PHP files in the `/var/www/html` directory or set up a new directory.
Create a Virtual Host configuration file (optional for custom domains):
```bash
sudo nano /etc/apache2/sites-available/yourdomain.conf
```
Restart Apache:
```bash
sudo systemctl restart apache2
```

##7. Set Up a Database 
Install MySQL or MariaDB for database support:
```bash
sudo apt install mysql-server -y
```

Secure your database installation:
```bash
sudo mysql_secure_installation
```
Create a database and user, then grant permissions:
```sql
CREATE DATABASE yourdbname;
CREATE USER 'yourusername'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON yourdbname.* TO 'yourusername'@'localhost';
FLUSH PRIVILEGES;
```

## 8. Upload Your PHP Files
Use SFTP or File Transfer Protocol (FTP) to upload your PHP files. You can use an FTP client like FileZilla:
Connect to your server using its IP, username, and password.
Upload files to the web root directory, usually `/var/www/html.`

## Enable HTTPS 
Install Certbot to add SSL for free with Let's Encrypt:
``` bash
sudo apt install certbot python3-certbot-apache  # For Apache
```
Run Certbot to automatically configure HTTPS:
```bash
sudo certbot --apache
```
