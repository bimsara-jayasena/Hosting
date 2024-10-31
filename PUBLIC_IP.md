# set up Apache to serve the site on that IP.

## Step 1: Configure Apache Virtual Host for Your IP Address

1. **Open Apache’s Default Virtual Host Configuration**
   - Apache usually comes with a default virtual host file. You can either edit it or create a new one:
     ```bash
     sudo nano /etc/apache2/sites-available/000-default.conf
     ```

2. **Set Up the Virtual Host to Use Your Public IP**
   - Modify the configuration to include your public IP address:
     ```apache
     <VirtualHost *:80>
         ServerAdmin webmaster@yourserver.com
         DocumentRoot /var/www/html  # Or the path to your project directory

         # This will allow access by IP
         ServerName your_public_ip

         ErrorLog ${APACHE_LOG_DIR}/error.log
         CustomLog ${APACHE_LOG_DIR}/access.log combined
     </VirtualHost>
     ```

   - Replace `your_public_ip` with the actual IP address of your server.
   - Ensure the `DocumentRoot` points to the directory where your project files are located.

3. **Enable the Virtual Host**
   - You might not need to enable it if it’s already the default. However, if you created a new file for this setup, enable it:
     ```bash
     sudo a2ensite 000-default.conf
     ```

4. **Reload Apache to Apply Changes**
   ```bash
   sudo systemctl reload apache2
   ```

### Step 2: Access Your Site by IP Address

Now you should be able to access your site directly by going to `http://your_public_ip` in your browser.

### Optional: Test Locally with `/etc/hosts` (for custom names)

If you want to test using a custom name without a DNS, you can modify your local machine's `hosts` file to map the IP to a name (e.g., `mywebsite.local`):

1. **Edit the Hosts File on Your Local Machine**
   - Open the hosts file:
     ```bash
     sudo nano /etc/hosts
     ```

2. **Add an Entry for Your Public IP**
   ```plaintext
   your_public_ip mywebsite.local
   ```

3. **Save and Exit**.
   - Now, you can type `http://mywebsite.local` in your browser to access the site, but this only works on the computer where you edited the `hosts` file.

This setup should allow you to access your PHP site through your public IP or locally through a custom name.
