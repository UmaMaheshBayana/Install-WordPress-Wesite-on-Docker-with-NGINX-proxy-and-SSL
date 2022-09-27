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

sudo mkdir wordpress




