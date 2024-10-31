# Hosting Multiple Sites by Domain/Subdomain (Using Virtual Hosts)

## 1.Set Up DNS

If you have domain names for each site, point each domain to your VPS IP address in your domain registrar's DNS settings (create an A record for each domain).
Create a Separate Directory for Each Site

Organize each website in its own directory within /var/www or wherever your web root is:
```bash

sudo mkdir -p /var/www/site1.com
sudo mkdir -p /var/www/site2.com
```
## 2.Set Up Virtual Host Files for Each Site

Create a new configuration file for each site in the `/etc/apache2/sites-available` directory.

### Example Configuration for Site 1:

```bash
sudo nano /etc/apache2/sites-available/site1.com.conf
```

```bash
<VirtualHost *:80>
    ServerAdmin admin@site1.com
    ServerName site1.com
    ServerAlias www.site1.com
    DocumentRoot /var/www/site1.com

    ErrorLog ${APACHE_LOG_DIR}/site1_error.log
    CustomLog ${APACHE_LOG_DIR}/site1_access.log combined
</VirtualHost>
```
### Example Configuration for Site 2:

```bash
sudo nano /etc/apache2/sites-available/site2.com.conf
```
```bash
<VirtualHost *:80>
    ServerAdmin admin@site2.com
    ServerName site2.com
    ServerAlias www.site2.com
    DocumentRoot /var/www/site2.com

    ErrorLog ${APACHE_LOG_DIR}/site2_error.log
    CustomLog ${APACHE_LOG_DIR}/site2_access.log combined
</VirtualHost>
```
##3.Enable the Virtual Hosts

###Enable each virtual host configuration file:
```bash

sudo a2ensite site1.com.conf
sudo a2ensite site2.com.conf
```
##4.Reload Apache

After enabling the new sites, reload Apache to apply the changes:
```bash
sudo systemctl reload apache2
```
##5.Access Each Site

Now, each site should be accessible through its own domain (e.g., http://site1.com and http://site2.com).
