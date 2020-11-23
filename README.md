# EASY TO SETUP MATOMO DOCKER-COMPOSE
Example repository with docker-compose file to quickly start your own Matomo setup. There is a version with and without ssl connection. If you wish to use the ssl connection you require an SSL certificate first. See [Paul Ridgway's blog](https://blockdev.io/quick-and-easy-lets-encrypt-certificates-using-docker/) on how to do so.

## Quick setup (No SSL)
Change the website url in `nginx/nginx.conf` to your host name (if running locally use `localhost`).

Start the docker-compose file and go to port 80:

```bash
docker-compose up
```

Click next and follow the installation steps, database info:

* Login = matomo
* Password = #your_secret_password
* Database Name = matomo

Now create your super user and add your first website!

## Almost as quick setup (SSL)
Request your ssl certificates via Let's Encrypt!:

```bash
sudo docker run -it --rm -p 443:443 -p 80:80 --name letsencrypt \
            -v "/etc/letsencrypt:/etc/letsencrypt" \
            -v "/var/lib/letsencrypt:/var/lib/letsencrypt" \
            <your-domain>/letsencrypt/letsencrypt:latest auth
```

Replace the `nginx.conf` and `docker-compose.yaml` file by those provided in the `ssl` directory.

```bash
rm docker-compose.yaml nginx/nginx.conf
mv ssl/docker-compose.yaml . && mv ssl/nginx.conf nginx
```

Change the website urls in `nginx/nginx.conf` to your host name and set the correct path to your ssl certificate in de `docker-compose.yaml` file (nginx volumes tab). Now you can start your server with docker-compose:

```bash
docker-compose up
```

Click next and follow the installation steps, database info:

* Login = matomo
* Password = #your_secret_password
* Database Name = matomo

Now create your super user and add your first website!