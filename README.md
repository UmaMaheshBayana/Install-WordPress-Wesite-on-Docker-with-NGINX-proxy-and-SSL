# Install WordPress Wesite on Docker with NGINX proxy and SSL

****Install Docker on the Ubuntu machine****  
```
curl -fsSL https://get.docker.com -o get-docker.sh
````
```
sh get-docker.sh
````
```
exit
````
Login Again

```
sudo usermod -aG docker $USER 
````
To check the Docker Containers
```
docker ps -a
````
If you get permission denied execute the below command
```
sudo chmod 666 /var/run/docker.sock
````

****Next Install Docker Compose****
```
sudo curl -L https://github.com/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
````
```
sudo chmod +x /usr/local/bin/docker-compose
````
```
docker-compose --version
````
***Create a dicrectory to store docker compose file.***

```
sudo mkdir wordpress
````
```
sudo nano docker-compose.yml
````

past the below line in the file and save the file.

```
version: "3"
services:
  mysql_db:
    container_name: mysql_container
    environment:
      MYSQL_DATABASE: wordpress_db
      MYSQL_PASSWORD: password
      MYSQL_ROOT_PASSWORD: password
      MYSQL_USER: admin
    image: "mysql:5.7"
    restart: always
    volumes:
      - "mysql:/var/lib/mysql"

  wordpress:
    container_name: wordpress_container
    depends_on:
      - mysql_db
    environment:
      WORDPRESS_DB_HOST: "mysql_db:3306"
      WORDPRESS_DB_NAME: wordpress_db
      WORDPRESS_DB_PASSWORD: password
      WORDPRESS_DB_USER: admin
    image: "wordpress:latest"
    ports:
      - "8080:80"
    restart: always
    volumes:
      - "./:/var/www/html"

volumes:
  mysql: {}

````
Connect to the wordpress website with the machine IP address and create admin account to login.

***Configure SSL certificate using letsEncrypt***

Install Nginx server

```
sudo apt install nginx
````
Next Create a configuration file in sites-available

```
cd /etc/nginx/sites-available
````
```
sudo vi yourdomain.conf
````

past the below lines in the configuration file

```
server {
root /var/www/html;
        listen 80; 
        listen [::]:80;
        server_name yourdomain.com www.yourdomain.com;
        location / {
            proxy_pass         http://127.0.0.1:8080;
            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Host $server_name;
            proxy_set_header   X-Forwarded-Proto $scheme;
        }
}
````

Go to sites enable and make yourdomain.conf as the default config file.

```
cd /etc/nginx/sites-enabaled
````

```
sudo ln -s /etc/nginx/sites-available/yourdomain.conf
````





