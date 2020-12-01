# Dockerized Matomo setup
Configuration for a dockerized Matomo setup using docker-compose. This docker-compose cofniguration in this repository takes care of your matomo server (incl. SSL connections). The only prerequisites are a working installation of [docker-compose]() and a server that can host your instance of Matomo.


In order to establish a secure connection you require an SSL certificate. We use Philipp Schmieder's excellent [nginx cerbot](https://github.com/wmnnd/nginx-certbot) to obtain and maintain our certificates.

## Installation

### 1. SSL
_Note: If you want to run matomo without SSL connection only replace 'example.org' in the `nginx/nginx.conf` file and skip to step 2._

If you wish to connect with your Matomo server via an SSL connection, replace the `docker-compose.yml` and `nginx/nginx.conf` files with those provided in the ssl directory:

```bash
rm docker-compose.yml nginx/nginx.conf
mv ssl/docker-compose.yml . && mv ssl/nginx.conf nginx
```

Change all occurences of 'example.org' in `nginx/nginx.conf` to your domain name (replace all), and add your domain and email adress to the `init-letsencrypt.sh` file. You can now request your certificates by running the script.

```bash
./ssl/init-letsencrypt.sh
```

### 2. Matomo
Start your matomo server by running docker-compose:

```bash
docker-compose up
```

During the GUI installation of Matomo, you're asked to connect to your database. If you have not changed any login information in the `setup-db.sql` file, you can use the following values to connect with your database:

* Login = matomo
* Password = #your_secret_password
* Database Name = matomo


Due to our nginx proxy setup, Matomo gets all its requests from the nginx server. In order to let matomo view the visitors IP, we need to add some lines to the matomo config. These lines are found in `conf/proxy_config` and can be automatically inserted by running:

```bash
./conf/proxy_config
```

That's it, you can now create your super user and add your first website!
