# DEPLOYING A LEMP STACK
**Prerequisites**

 We need access to an Ubuntu 20.04 server as a regular, non-root sudo user, and a firewall enabled on the server

*Step 1 – Installing the Nginx Web Server*

 We are going to employ Nginx, a high-performance web server to display web pages to our site visitors.

 We start off by updating your server’s package index. Following that, you can use apt install to get Nginx installed

 `$ sudo apt update`

`$ sudo apt install nginx`

![Package update](images/lemp1.jpg)

![Nginx installation](images/lemp2.jpg)

To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

`$ sudo systemctl status nginx`

If it is green and running, then everything was done correctly

![Nginx running](images/lemp4.jpg)

 let us try to check how we can access it locally in our Ubuntu shell, run:

 `$ curl http://localhost:80`

 ![Nginx](images/lemp5.jpg)

 Now we can test how our Nginx server can respond to requests from the Internet. Open a web browser and try to access this url


`http://<Public-IP-Address>:80`

![Nginx running](images/lemp6.jpg)

*Step 2 — Installing MySQL*

Now that we have a web server up and running, we need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database.

Again, use apt to acquire and install MySQL:

`$ sudo apt install mysql-server`

![Sql install](images/lemp7.jpg)

When prompted, confirm installation by typing Y, and then ENTER.

When the installation is finished, run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to the database system. Start the interactive script by running:

`$ sudo mysql_secure_installation`

When finished, test if you’re able to log in to the MySQL console by typing:

`$ sudo mysql`

![Sql running](images/lemp8.jpg)

*Step 3 – Installing PHP*

we need to install PHP to process code and generate dynamic content for the web server.

we’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

To install these 2 packages at once, run:

`$ sudo apt install php-fpm php-mysql`

![Php dependencies](images/lemp9.jpg)

When prompted, type Y and press ENTER to confirm installation.

You now have your PHP components installed. Next, you will configure Nginx to use them.


*Step 4 — Configuring Nginx to Use PHP Processor*

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at `/var/www/html`. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying `/var/www/html`, we’ll create a directory structure within `/var/www` for the **your_domain** website, leaving `/var/www/html` in place as the default directory to be served if a client request does not match any other sites.

Create the root web directory for **your_domain** as follows:

`sudo mkdir /var/www/projectLEMP`

we then assign ownership of the directory with the `$USER` environment variable, which will reference your current system user:

`$ sudo chown -R $USER:$USER /var/www/projectLEMP`

We then, open a new configuration file in Nginx’s `sites-available` directory using Nano editor

 `sudo nano /etc/nginx/sites-available/projectLEMP`

 This will create a new blank file. Paste in the following bare-bones configuration

 server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

![Php dependencies](images/lemp10.jpg)

Activate your configuration by linking to the config file from Nginx’s `sites-enabled` directory:

`$ sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

You can test your configuration for syntax errors by typing:

`$ sudo nginx -t`

![Php dependencies](images/lemp11.jpg)

We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

`sudo unlink /etc/nginx/sites-enabled/default`

reload Nginx to apply the changes:

`$ sudo systemctl reload nginx`

![Php dependencies](images/lemp12.jpg)

The new website is now active, but the web root `/var/www/projectLEMP` is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:


`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

Now go to your browser and try to open your website URL using IP address:

`http://<Public-IP-Address>:80`

The LEMP stack is now fully configured

*Step 5 – Testing PHP with Nginx*


To validate that Nginx can correctly hand `.php` files off to your PHP processor.

we create a test PHP file in our document root. Open a new file called `info.php` within your document root in your text editor:

`$ nano /var/www/projectLEMP/info.php`


Type the following lines into the new file

`<?php`

 `phpinfo();`

 We can now access this page in the web browser by visiting the domain name or public IP address we’ve set up in our Nginx configuration file, followed by `/info.php`:

 `http://`server_domain_or_IP`/info.php`

 ![info.php edit](images/lemp14.jpg)

 ![nano editor](images/lemp15.jpg)

 ![webpage](images/lemp16.jpg)



*Step 6 — Retrieving data from MySQL database with PHP*


We will create a database named example_database and a user named example_user, but you can replace these names with different values.

First, connect to the MySQL console using the root account:

`$ sudo mysql`

![mysql](images/lemp18.jpg)

To create a new database, run the following command from your MySQL console:

`CREATE DATABASE `example_database`;`

![mysql database](images/lemp19.jpg)


Now you can create a new user and grant him full privileges on the database you have just created.

`mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

Now we need to give this user permission over the example_database database:

`GRANT ALL ON example_database.* TO 'example_user'@'%';`

![mysql database](images/lemp20.jpg)

This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.

Now exit the MySQL shell with:

`mysql> exit`

You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

`$ mysql -u example_user -p`

![mysql database](images/lemp21.jpg)

`mysql> SHOW DATABASES;`


![mysql database](images/lemp22.jpg)


Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:



CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );


![mysql database](images/lemp23.jpg)

Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

To confirm that the data was successfully saved to your table, run:


`mysql>  SELECT * FROM example_database.todo_list;`

![mysql database](images/lemp24.jpg)

After confirming that you have valid data in your test table, you can exit the MySQL console:


`mysql> exit`


Now you can create a PHP script that will connect to MySQL and query for your content


`$ nano /var/www/projectLEMP/todo_list.php`


The following PHP script connects to the MySQL database and queries for the content of the todo_list table, displays the results in a list. If there is a problem with the database connection, it will throw an exception.

Copy this content into your `todo_list.php `script:

![mysql database](images/lemp25.jpg) 


Save and close the file when you are done editing.

You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by `/todo_list.php:`


`http://<Public_domain_or_IP>/todo_list.php`