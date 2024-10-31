## SETUP DNS
### Step 1: Point the Domain to Your Server’s IP Address

1. **Log in to your domain registrar’s website** (where you purchased the domain).
2. **Find the DNS settings** for your domain, often under a section like "DNS Management" or "Domain Settings."
3. **Create an A Record** pointing to your server’s IP address:
   - **Type**: A
   - **Name**: `@` (or leave blank for the root domain, e.g., `example.com`)
   - **Value**: Your server’s IP address (e.g., `192.0.2.123`)
   - **TTL**: Leave as default or set to a low value like 300 seconds (for quicker propagation)

4. Optionally, create a `www` subdomain to redirect as well:
   - **Type**: CNAME
   - **Name**: `www`
   - **Value**: Your root domain (e.g., `example.com`)

### Step 2: Configure Apache Virtual Host for Your Domain

1. **Create a Virtual Host file** for your domain:
   ```bash
   sudo nano /etc/apache2/sites-available/yourdomain.com.conf
   ```

2. **Set Up the Virtual Host** configuration:
   - Replace `yourdomain.com` with your actual domain name and `/var/www/yourdomain.com` with your site’s directory.
   - Example configuration:
     ```apache
     <VirtualHost *:80>
         ServerAdmin webmaster@yourdomain.com
         ServerName yourdomain.com
         ServerAlias www.yourdomain.com
         DocumentRoot /var/www/yourdomain.com

         ErrorLog ${APACHE_LOG_DIR}/yourdomain.com_error.log
         CustomLog ${APACHE_LOG_DIR}/yourdomain.com_access.log combined
     </VirtualHost>
     ```

3. **Enable the Virtual Host**:
   ```bash
   sudo a2ensite yourdomain.com.conf
   ```

4. **Reload Apache** to apply the changes:
   ```bash
   sudo systemctl reload apache2
   ```

### Step 3: Set Up SSL (Optional but Recommended)

For HTTPS, you can use **Certbot** to get a free SSL certificate from Let's Encrypt:

1. **Install Certbot** (if not already installed):
   ```bash
   sudo apt install certbot python3-certbot-apache
   ```

2. **Generate SSL Certificate**:
   ```bash
   sudo certbot --apache -d yourdomain.com -d www.yourdomain.com
   ```

   Follow the prompts to set up HTTPS. Certbot will automatically configure Apache with SSL.

### Step 4: Test the Configuration

- Visit your domain in a browser (`http://yourdomain.com` or `https://yourdomain.com` if you set up SSL) to confirm it’s working.

### Troubleshooting Tips
- Ensure your DNS changes have propagated; it can take a few minutes to several hours.
- Check Apache’s syntax with:
  ```bash
  sudo apache2ctl configtest
  ```
  
Following these steps should connect your domain to your site hosted on Apache!
