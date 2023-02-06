## Step Setup

`git clone https://github.com/dearvn/deploy-wp-2023.git wordpress`

`cd wordpress`

*Search and change your_domain to domain in file:*

`vi nginx-conf/nginx.conf`

`vi docker-compose.yml`

*Start your containers*

`docker-compose up -d`

*Check the status of your services*

`docker-compose ps`

*Check the service logs*

`docker-compose logs [service_name]`

*Check that your certificates have been mounted to the webserver*

`docker-compose exec webserver ls -la /etc/letsencrypt/live`

*Finish the installation through the WordPress web interface*

https://your_domain

![Alt text](https://github.com/dearvn/deploy-wp-2023/raw/main/setup.png?raw=true "Setup")







