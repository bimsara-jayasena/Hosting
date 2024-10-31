# Deploying a Laravel project on Hostinger VPS hosting 
### Step 1: Access Your VPS

1. **SSH into Your VPS**:
   - Use an SSH client (like PuTTY on Windows or Terminal on macOS/Linux) to connect to your VPS.
   - Run the following command, replacing `username` and `ip-address` with your credentials:
     ```bash
     ssh username@ip-address
     ```

### Step 2: Update Your Server

1. **Update the Package Manager**:
   ```bash
   sudo apt update
   sudo apt upgrade
   ```

### Step 3: Install Required Software

1. **Install Nginx or Apache** (choose one):
   - For **Nginx**:
     ```bash
     sudo apt install nginx
     ```
   - For **Apache**:
     ```bash
     sudo apt install apache2
     ```

2. **Install PHP**:
   - Install PHP along with necessary extensions:
     ```bash
     sudo apt install php php-fpm php-mysql php-xml php-mbstring php-zip php-curl php-gd
     ```

3. **Install Composer** (for Laravel dependencies):
   ```bash
   sudo apt install composer
   ```

4. **Install MySQL** (if your project uses a database):
   ```bash
   sudo apt install mysql-server
   ```

### Step 4: Upload Your Laravel Project

1. **Upload Files Using SFTP or SCP**:
   - You can use an SFTP client like FileZilla or WinSCP to upload your Laravel project files to the server.
   - Alternatively, you can use SCP:
     ```bash
     scp -r /path/to/your/laravel/project username@ip-address:/var/www/html/
     ```
   - Ensure your project is in the correct directory, like `/var/www/html/laravel-project`.

### Step 5: Set Permissions

1. **Set Proper Permissions**:
   ```bash
   sudo chown -R www-data:www-data /var/www/html/laravel-project
   sudo chmod -R 755 /var/www/html/laravel-project/storage
   sudo chmod -R 755 /var/www/html/laravel-project/bootstrap/cache
   ```

### Step 6: Configure Environment Variables

1. **Edit `.env` File**:
   - Navigate to your project directory and edit the `.env` file:
     ```bash
     cd /var/www/html/laravel-project
     nano .env
     ```
   - Update the database connection details, application URL, and any other necessary environment settings.

### Step 7: Run Composer Install

1. **Install Laravel Dependencies**:
   ```bash
   composer install --no-dev
   ```

2. **Generate Application Key**:
   ```bash
   php artisan key:generate
   ```

### Step 8: Configure Your Web Server

1. **For Nginx**:
   - Create a new configuration file:
     ```bash
     sudo nano /etc/nginx/sites-available/laravel-project
     ```
   - Add the following configuration:
     ```nginx
     server {
         listen 80;
         server_name example.com; # Your domain name or IP

         root /var/www/html/laravel-project/public;
         index index.php index.html index.htm;

         location / {
             try_files $uri $uri/ /index.php?$query_string;
         }

         location ~ \.php$ {
             include snippets/fastcgi-php.conf;
             fastcgi_pass unix:/var/run/php/php7.x-fpm.sock; # Replace with your PHP version
             fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
             include fastcgi_params;
         }

         location ~ /\.ht {
             deny all;
         }
     }
     ```

   - Enable the site and test the configuration:
     ```bash
     sudo ln -s /etc/nginx/sites-available/laravel-project /etc/nginx/sites-enabled/
     sudo nginx -t
     ```

2. **For Apache**:
   - Create a new configuration file:
     ```bash
     sudo nano /etc/apache2/sites-available/laravel-project.conf
     ```
   - Add the following configuration:
     ```apache
     <VirtualHost *:80>
         ServerAdmin admin@example.com
         DocumentRoot /var/www/html/laravel-project/public
         ServerName example.com

         <Directory /var/www/html/laravel-project>
             AllowOverride All
         </Directory>

         ErrorLog ${APACHE_LOG_DIR}/error.log
         CustomLog ${APACHE_LOG_DIR}/access.log combined
     </VirtualHost>
     ```

   - Enable the site and the rewrite module:
     ```bash
     sudo a2ensite laravel-project.conf
     sudo a2enmod rewrite
     ```

### Step 9: Restart the Web Server

1. **Restart Nginx or Apache**:
   - For Nginx:
     ```bash
     sudo systemctl restart nginx
     ```
   - For Apache:
     ```bash
     sudo systemctl restart apache2
     ```

### Step 10: Set Up Your Database (if needed)

1. **Create Database and User**:
   - Log into MySQL:
     ```bash
     sudo mysql -u root -p
     ```
   - Run the following commands in the MySQL shell:
     ```sql
     CREATE DATABASE your_database_name;
     CREATE USER 'your_user'@'localhost' IDENTIFIED BY 'your_password';
     GRANT ALL PRIVILEGES ON your_database_name.* TO 'your_user'@'localhost';
     FLUSH PRIVILEGES;
     ```

2. **Run Migrations**:
   ```bash
   php artisan migrate
   ```

### Step 11: Test Your Deployment

1. **Access Your Project**:
   - Open a web browser and go to `http://your-domain.com` or `http://ip-address`.

2. **Check for Errors**:
   - If there are any issues, check the logs for troubleshooting:
     - Nginx: `/var/log/nginx/error.log`
     - Apache: `/var/log/apache2/error.log`
     - Laravel: `storage/logs/laravel.log`

### Conclusion

Your Laravel project should now be live on your Hostinger VPS! Make sure to replace any placeholder text with your actual project details and credentials. If you encounter any issues, consult the server logs for more information or troubleshooting tips.
