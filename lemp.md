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

