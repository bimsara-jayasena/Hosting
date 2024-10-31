
### Example Configuration for Multiple Sites in a Single File


1. **Create a Single Configuration File**
   - You can create a configuration file (e.g., `myapp.conf`) in the `/etc/apache2/sites-available/` directory.

2. **Example Configuration File:**
   ```apache
   <VirtualHost *:80>
       ServerName server-public-ip  # Use your actual public IP
       DocumentRoot /var/www/myapp    # Common DocumentRoot for all sites

       ErrorLog ${APACHE_LOG_DIR}/app_error.log
       CustomLog ${APACHE_LOG_DIR}/app_access.log combined

       <Directory /var/www/myapp>
           Options Indexes FollowSymLinks
           AllowOverride All
           Require all granted
       </Directory>
   </VirtualHost>
   ```

3. **Application-Level Routing**
   In your application (like a Laravel or any other PHP framework), you would manage routing based on the paths. For example:

   - `http://server-public-ip/site1` could route to `site1` logic.
   - `http://server-public-ip/site2` could route to `site2` logic.

### Enabling the Configuration

1. **Enable the Site:**
   After creating the configuration file, enable it using:
   ```bash
   sudo a2ensite myapp.conf
   ```

2. **Reload Apache:**
   Finally, reload Apache to apply the changes:
   ```bash
   sudo systemctl reload apache2
   ```
