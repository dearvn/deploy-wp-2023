## Step Setup

`git clone https://github.com/dearvn/deploy-wp-2023.git wordpress`

`cd wordpress`

# STEP 1 => deploy on http://
*Search and change your_domain to your domain in file*

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

**if step 1 is ok then move to step 2 https**

# STEP 2 => deploy on https://

`vi docker-compose.yml`

*Find the section of the file with the certbot service definition, and replace the --staging flag in the command option with the --force-renewal flag*

`docker-compose up --force-recreate --no-deps certbot`

`docker-compose stop webserver`

`curl -sSLo nginx-conf/options-ssl-nginx.conf https://raw.githubusercontent.com/certbot/certbot/master/certbot-nginx/certbot_nginx/_internal/tls_configs/options-ssl-nginx.conf`

`cp ssl-nginx-conf/ssl-nginx.conf nginx-conf/nginx.conf`

*One time, search and change your_domain to your domain in nginx.conf file*

`vi docker-compose.yml`

*In the webserver service definition, add the following port mapping below line: - "80:80"*

`- "80:80"`
`- "443:443"`

*Then, recreate the webserver service*

`docker-compose up -d --force-recreate --no-deps webserver`

*Check your services with docker-compose ps*

`docker-compose ps`

*You will also include the --no-deps option to tell Compose that it can skip starting the webserver service, since it is already running*

*Finish the installation through the WordPress web interface*

https://your_domain

![Alt text](https://github.com/dearvn/deploy-wp-2023/raw/main/setup.png?raw=true "Setup")



## Renew ssl

*Change to the ~/wordpress project directory*

`vi ssl_renew.sh`

*Make it executable with the following command*

`chmod +x ssl_renew.sh`

*Open your root crontab file to run the renewal script at a specified interval*

`sudo crontab -e`

`0 12 * * * /home/admin/wordpress/ssl_renew.sh >> /var/log/cron.log 2>&1`

