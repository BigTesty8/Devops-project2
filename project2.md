# STEP 1 – INSTALLING THE NGINX WEB SERVER
- Launch an instance > install and use GITBASH
- To install NGINX web server use the > sudo apt update and also sudo apt install nginx command to install nginx.
- To verify > sudo systemctl status nginxand to access host server use the > curl http ://Localhost80.
- Copy public IP address and paste on a search engine and you would get WELCOME TO NGINX
- ![Screenshot 2023-06-27 185558](https://github.com/BigTesty8/Devops-project2/assets/137091610/b9c9db70-301f-4bc9-b0bd-9aaa7f494340)
- # STEP 2 — INSTALLING MYSQL
- To install mysql you need to run the following commands below > STEP 2 — INSTALLING MYSQL > sudo mysql > exit
- # INSTALLING PHP
- To install these two packages at once run the following commands > sudo apt install php-fpm php-mysql .
- When prompted, type Y and press ENTER to confirm installation.
You now have your PHP components installed. Next, you will configure Nginx to use them.
# STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR
Create the root web directory for your_domain as follows: > sudo mkdir /var/www/projectLEMP to change user run > sudo chown -R $USER:$USER /var/www/projectLEMP To open editor run > sudo nano /etc/nginx/sites-available/projectLEMP and insert this command >>
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
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }
    To link the config file to ngnix run > sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
To test for erros run > sudo nginx -t
We also need to disable default Nginx host that is currently configured to listen on port 80, for this run: sudo unlink /etc/nginx/sites-enabled/default .
When you are ready, reload Nginx to apply the changes:sudo systemctl reload nginx
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
Now go to your browser and try to open your website URL using IP address: http://<Public-IP-Address>:80 
![Screenshot 2023-06-28 070021](https://github.com/BigTesty8/Devops-project2/assets/137091610/e3b37613-3b2b-423f-8500-ba7ab8dd3704)
# STEP 5 – TESTING PHP WITH NGINX
to do this open a file with thr following commands > sudo nano /var/www/projectLEMP/info.php > input this valid PhP code <?php
phpinfo(); You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:http://`server_domain_or_IP`/info.php .
![Screenshot 2023-06-28 071346](https://github.com/BigTesty8/Devops-project2/assets/137091610/ccd25a69-a37c-47e3-9c24-be00f7c2b2cb)
# STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)
First, connect to the MySQL console using the root account:sudo mysql > create a data base using > mysql> CREATE DATABASE `example_database`; to grant the user permission run > GRANT ALL ON example_database.* TO 'example_user'@'%'; > mysql> exit to exit the sql console > mysql -u example_user -p > SHOW DATABASES; .
Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:
CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
>  INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
To confirm that the data was successfully saved to your table, run: SELECT * FROM example_database.todo_list;
> After confirming that you have valid data in your test table, you can exit the MySQL console: mysql> exit
> Copy this content into your todo_list.php script:
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
Save and close file .
You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:
http://<Public_domain_or_IP>/todo_list.php
You should see a page like this, showing the content you’ve inserted in your test table.
![Screenshot 2023-06-28 075247](https://github.com/BigTesty8/Devops-project2/assets/137091610/841ac4b2-7eb6-41a1-ab4b-3d1914de6a73)


























