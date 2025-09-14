## Infrastructure plan for my VPS setup
  1. Host multiple Laravel apps under different subdomains
  2. Run different PHP versions per project
  3. Also host Node.js apps alongside them
  4. Keep things organized and production-ready

  ### 1. Server Setup Overview
  
  - OS: Ubuntu 22.04 LTS (your case).
  - SSH Access: Your laptop’s SSH key will be added to VPS (~/.ssh/authorized_keys).
  - Web Server: Use Nginx (preferred for Laravel + Node.js reverse proxy).
  - Process Manager:
    - PHP apps → php-fpm (per PHP version)
    - Node.js apps → pm2 (keeps node apps alive)
  - Database: MySQL/MariaDB or PostgreSQL (if needed).
  - SSL: Let’s Encrypt (using Certbot).
      
### 2. Directory Structure

  Keep apps under /var/www/ for organization:
  ```
  /var/www/
   ├── laravel-app1/
   │     └── public/ (Laravel public folder)
   ├── laravel-app2/
   │     └── public/
   ├── node-app1/
   ├── node-app2/
  
  ```
  
Each project has its own folder.

### 3. PHP Version Management

  Install multiple PHP versions (e.g., PHP 7.4, 8.1, 8.2) with php-fpm.
  Each app can run on a different PHP socket. Example:
    - Laravel App1 → PHP 8.1 (`/run/php/php8.1-fpm.sock`)
    - Laravel App2 → PHP 8.2 (`/run/php/php8.2-fpm.sock`)

  Nginx will point each app to its matching PHP socket.

### 4. Node.js Projects
  - Use NVM (Node Version Manager) to manage multiple Node versions per project.
  - Use PM2 to run Node apps in the background.
  - Nginx will reverse-proxy requests:
    - node-app1.domain.com → proxy → localhost:3001
    - node-app2.domain.com → proxy → localhost:3002

### 5. Domain & Subdomains

  Example setup (DNS):
  ```
    app1.yourdomain.com   → VPS IP
    app2.yourdomain.com   → VPS IP
    node1.yourdomain.com  → VPS IP
    node2.yourdomain.com  → VPS IP
  ```

  On Nginx, create separate server blocks:
  ```
  /etc/nginx/sites-available/

   ├── laravel-app1.conf
   ├── laravel-app2.conf
   ├── node-app1.conf
   ├── node-app2.conf
  ```
Then symlink them to sites-enabled/.

### 6. SSL Certificates

    - Use Certbot with Nginx plugin.
    - Each subdomain gets a free SSL from Let’s Encrypt.
    - Auto-renewal every 90 days.




  







