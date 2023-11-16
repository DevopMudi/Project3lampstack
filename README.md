# Project 3 LAMP stack
Step 0 – Preparing prerequisites

In order to complete this project we will need an AWS account and a virtual server with Ubuntu Server OS.
Watch the video in the following steps to help you set up your account
- 1.	AWS account setup and Provisioning an Ubuntu Server [visit aws.amazon.com](https://www.youtube.com/watch?v=xxKuB9kJoYM&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=6)
  2. [Connecting to your EC2 Instance](https://www.youtube.com/watch?v=TxT6PNJts-s&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=7)

Note : Ensure to always stop your instance or terminate it when you are done working with, and also you are always on the free tier of whatever you do to avoid charges.
All we need to know right now is that we can use 750 hours (31.25 days) of t2.micro server per month for the first 12 months FOR FREE.
Access the terminal on your laptop using gitbash or visual studio code and follow the steps below
From your terminal connect to your EC2 instance as earlier discribe in the video through ssh and follow the steps below as in the picture below:

![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/c37cad61-0142-4fa7-be28-17c9898626ed)
You should see Ubuntu like in the picture below which confirm you have been connected
![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/a52f2303-92af-463b-88ca-b59565394b1e)

AFTER CONNECTING TO YOUR EC2 FOLLOW THE BELOW STEPS

STEP 1 — INSTALLING APACHE AND UPDATING THE FIREWALL

- 1. Update a list of packages in package manager by running the command: **sudo apt update**
  2. Run apache2 package installation by running the command: **sudo apt install apache2**
  3. To verify that apache2 is running as a Service in our OS, use following command: **sudo systemctl status apache2**
  4. 

![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/1df4237f-f878-4708-b481-faa67bb21f41)

![image009](https://github.com/DevopMudi/Project3lampstack/assets/149855241/212f7e5a-ee3b-44cf-8b0a-0491d22685fc)

If it is green and running(showing active running as in the above picture), then you did everything correctly – you have just launched your first Web Server in the Clouds!
Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet
As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80:

![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/f60de56a-9013-471e-9d81-6cbcefa6bfa1)

Adding rule in the edit in-bound rule and save rule

![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/37691558-de72-499a-8daa-9dc2740967ef)

![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/8b98c3e7-ab0b-41cc-a7c5-8648b5a74baa)


Our server is now running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).
First, let us try to check how we can access it locally in our Ubuntu shell, run:
 curl http://localhost:80
 
or

 curl http://127.0.0.1:80
 
 or Click open any browser of your choice and type in : http://16.16.182.107/80
 You should see the below image when open on the browser.

 
 ![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/d0c5517d-bb7a-4335-b217-752a65732bbc)



 STEP 2 — INSTALLING MYSQL
 
- Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in
- a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.
  Again, use ‘apt’ to acquire and install this software: **sudo apt install mysql-server**

- When prompted, confirm installation by typing Y, and then ENTER. When the installation is finished, log in to the MySQL console by typing: **sudo mysql**
- This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command.
- You should see output like this image below:

- ![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/e4bd5c24-a7f4-456a-b612-653255b74ed0)

- It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.1.
**ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';**


- This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.
Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.
Answer Y for yes, or anything else to continue without enabling.

![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/1bb1a691-45c6-4628-8931-7e30869643b8)

VALIDATE PASSWORD PLUGIN can be used to test passwords and improve security. It checks the strength of password and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:

If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words e.g PassWord.1.
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1

- Regardless of whether you chose to set up the VALIDATE PASSWORD PLUGIN, your server will next ask you to select and confirm a password for the MySQL root user. This is not to be confused with the system root. The database root user is an administrative user with full privileges over the database system. Even though the default authentication method for the MySQL root user dispenses the use of a password, even when one is set, you should define a strong password here as an additional safety measure. We’ll talk about this in a moment.
If you enabled password validation, you’ll be shown the password strength for the root password you just entered and your server will ask if you want to continue with that password. If you are happy with your current password, enter Y for “yes” at the prompt:
Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y

- For the rest of the questions, press Y and hit the ENTER key at each prompt. This will prompt you to change the root password, remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.
When you’re finished, test if you’re able to log in to the MySQL console by typing: **sudo mysql -p**

Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.To exit the MySQL console, type: **mysql> exit**
Notice that you need to provide a password to connect as the root user.
- For increased security, it’s best to have dedicated user accounts with less expansive privileges set up for every database, especially if you plan on having multiple databases hosted on your server.
Note: At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. For that reason, when creating database users for PHP applications on MySQL 8, you’ll need to make sure they’re configured to use mysql_native_password instead.
Your MySQL server is now installed and secured. Next, we will install PHP, the final component in the LAMP stack.

STEP 3 — INSTALLING PHP

- You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
To install these 3 packages at once, run:  **sudo apt install php libapache2-mod-php php-mysql**
- Once the installation is finished, you can run the following command to confirm your PHP version:  **php -v**


![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/fe8af12c-5c06-4e46-9ac6-2d57c7036c48)

![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/6c62631b-3b67-4f23-b54b-cd1678b6904f)

STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE

- In this project, you will set up a domain called projectlamp, but you can replace this with any domain of your choice.
Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory.
We will leave this configuration as is and will add our own directory next next to the default one.
Create the directory for projectlamp using ‘mkdir’ command as follows:follow through image and code in image illustrated below.

![image023](https://github.com/DevopMudi/Project3lampstack/assets/149855241/c230302b-3241-4db4-b3c7-13c73d9b07dc)


STEP 5 — **ENABLE PHP ON THE WEBSITE**
- With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.
In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:
sudo vim /etc/apache2/mods-enabled/dir.conf
![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/1a1b146b-3e63-4863-ba1b-51e7c6b8092b)

After saving and closing the file, you will need to reload Apache so the changes take effect: **sudo systemctl reload apache2**

- Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server.
Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
Create a new file named index.php inside your custom web root folder: **vim /var/www/projectlamp/index.php**


This will open a blank file. Add the following text, which is valid PHP code, inside the file:

![image](https://github.com/DevopMudi/Project3lampstack/assets/149855241/d804d215-19bc-40f4-b041-8dfa301a1900)


When you are finished, save and close the file, refresh the page and you will see a page similar to this:

![image025](https://github.com/DevopMudi/Project3lampstack/assets/149855241/054586b7-ef9a-4322-a884-7eb3a492b770)


![image027](https://github.com/DevopMudi/Project3lampstack/assets/149855241/0b694b54-e3ef-478d-b8d3-826fd62c62fd)



- The above image page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.
If you can see this page in your browser, then your PHP installation is working as expected.


 




 




















