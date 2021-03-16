# DEPLOYING A LEMP STACK
**Prerequisites**

 We need access to an Ubuntu 20.04 server as a regular, non-root sudo user, and a firewall enabled on the server

*Step 1 – Installing the Nginx Web Server*

 We are going to employ Nginx, a high-performance web server to display web pages to our site visitors.

 We start off by updating your server’s package index. Following that, you can use apt install to get Nginx installed

 `$ sudo apt update`

`$ sudo apt install nginx`

![Package update](lemp1.jpg)

![Nginx installation](lemp2.jpg)

To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

`$ sudo systemctl status nginx`

If it is green and running, then everything was done correctly

![Nginx running](lemp4.jpg)

 let us try to check how we can access it locally in our Ubuntu shell, run:

 `$ curl http://localhost:80`

 ![Nginx](lemp5.jpg)

 Now we can test how our Nginx server can respond to requests from the Internet. Open a web browser and try to access this url


`http://<Public-IP-Address>:80`

![Nginx running](lemp6.jpg)

*Step 2 — Installing MySQL*

Now that we have a web server up and running, we need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database.

Again, use apt to acquire and install MySQL:

`$ sudo apt install mysql-server`

![Sql install](lemp7.jpg)

When prompted, confirm installation by typing Y, and then ENTER.

When the installation is finished, run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to the database system. Start the interactive script by running:

`$ sudo mysql_secure_installation`

When finished, test if you’re able to log in to the MySQL console by typing:

`$ sudo mysql`

![Sql running](lemp8.jpg)

*Step 3 – Installing PHP*

we need to install PHP to process code and generate dynamic content for the web server.

we’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

To install these 2 packages at once, run:

`$ sudo apt install php-fpm php-mysql`

![Php dependencies](lemp9.jpg)

When prompted, type Y and press ENTER to confirm installation.

You now have your PHP components installed. Next, you will configure Nginx to use them.


