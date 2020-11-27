

Introduction

 

Official support website: https://www.llstack.com/ols/

 

OLStack Community Container Edition is an OpenLiteSpeed environment based on Docker containerized orchestration . It has better performance than Nginx and is basically compatible with the Apache HTTPD ecosystem. It mainly does not support automatic loading of .htaccss files. This version has no restrictions on the operating system environment and can be applied to many scenarios in the future.

 

OpenLiteSpeed is the community version of LiteSpeed ​​EnterPrise . Compared with Nginx, many extensions, such as Brotli , nginx*-cache-*purge, etc., will not support the latest stable version due to untimely updates , and there is no enterprise-level guarantee. The OpenLiteSpeed components of official conduct major maintenance and updating, providing commercial enterprise-class experience.

 

In performance, LiteSpeed Tech provided by BenchMark , the WordPress , Joomla , OpenCart , ModSecurity , small static files, HTTP / 2 , HTTP / 3 on the test than the Apache HTTPD and Nginx have it perform better, not just It is to run a Hello World but to conduct a complete test.

 

This is a Fork of litespeedtech/ols-docker-env .

Installation Environment

Domestic server preparation environment

 

1. Install the Docker environment, you can skip it already

 

curl -sSL https://get.daocloud.io/docker | sh  

 

2. Install the Docker-Compose environment, 1.25.3 can be modified according to the latest version , and can be skipped

 

curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.3/docker-compose-`uname -s`-`uname -m`> /usr/local/bin/docker- compose

chmod +x /usr/local/bin/docker-compose

 

Three, download OLStack

 

## no download git can be prepared by apt install git -y or -y yum install git install

git clone https://gitee.com/LLStack/OLStack.git

cd OLStack

 

Overseas server preparation environment

 

1. Install the Docker environment, you can skip it already

 

curl -s https://get.docker.com | sudo sh

 

2. Install Docker-Compose environment, 1.25.4 can be modified according to the latest version , and it can be skipped

 

curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

 

Three, download OLStack

 

## no download git can be prepared by apt install git -y or -y yum install git install

git clone https://github.com/LLStack/OLStack.git

cd OLStack

 

Edit configuration file

 

4. Edit the .env and docker-compose.yml files:

 

The .env file contains definitions for some versions. You can see the explanation of .env for details.

 

The docker-compose.yml file defines what container components are specifically installed, including Redis , phpmyadmin, etc. You can see the analysis of docker-compose.yml .

 

vi .env

vi docker-compose.yml

 

::: tip Tips for students who don’t know vi , you can use FileZilla , XFTP and other software that supports SFTP to download and edit the file before uploading it. :::

 

Five, start the container

 

docker-compose up -d

 

6. Start-up instructions:

 

docker-compose up ## Start all containers temporarily

docker-compose up -d ## Persistently start all containers

docker-compose stop ## Stop container operation

docker-compose down ## Stop and delete all containers

 

Configuration instructions

.ENV configuration

 

.env file contains definitions for some versions, because it is . beginning of the file, possibly in the portion of the computer display is hidden, it is necessary to open the Show hidden files.

 

described as follows:

 

1. Time zone setting, define the time zone of the business. The default is Asia/Shanghai . For example, if the service is in the eastern United States, you can modify it to America/New_York New York time.

 

TimeZone=Asia/Shanghai

 

2. OpenLiteSpeed version, currently OLS provides two versions 1.6.X and 1.5.X , and more versions may be provided in the future.

 

LITESPEED=ols1.6.9

 

Options available for modification: ols1.6.9 , ols1.5.11 and above, version view:

 

https://openlitespeed.org/release-log/

 

Three, PHP version by LiteSpeed support the official LSPHP version, and many companies use web hosting version is the same.

 

PHPVER=php73

 

Currently available: php74 , php73 , php72 , php71 , php70 , php56 , php55 , php54 , php53

 

Different versions of PHP are based on different versions of Ubuntu .

 

    The underlying system for php70 ~ 74 is Ubuntu 18.04 .

    The underlying system of php54 ~ 56 is Ubuntu 16.04 . PHP is not officially supported

    The underlying system of php53 is Ubuntu 14.04 . PHP and the system are not officially supported, only testing is recommended

 

::: tip prompts that the official life support cycle of each version of PHP is three years. If the program supports it, it is recommended to install the latest version to check the PHP version support: http://php.net/supported-versions.php :::

 

Fourth, MySQLTYPE , the type of running database.

 

MySQLTYPE=mariadb

 

Modifiable options: mariadb , percona

 

MariaDB and Percona are more developed and provide more functional options than the default MySQL .

 

5. MySQLVER , the specific version of the database.

 

MySQLVER=10.3

 

MariaDB currently provides: 10.4 , 10.3 , 10.2 (compatible with MySQL5.7 ) 10.1 (compatible with MySQL5.6 )

 

Percona currently provides: 8.0 , 5.7 , 5.6 (this compatibility relationship, not to mention you also know)

 

::: tip prompted due to the Docker convenience of the container, if you need PostGreSQL , SQL Server , MongoDB , elasticsearch can be modified directly docker-compose.yml file to achieve. :::

 

Six, the default database and user created

 

## Default database name

MYSQL_DATABASE=llstack

## Default database root account password

MYSQL_ROOT_PASSWORD=password

## Default new username

MYSQL_USER=llstack

## Default new user password

MYSQL_PASSWORD=password

 

Seven, REDIS_VERSION , Redis version configuration

 

REDIS_VERSION=5.0-alpine

 

Options available for modification: 6.0-rc-alpine , 5.0-alpine , 4.0-alpine , 3.2-alpine , 2.8

 

8. DOMAIN , the default domain name, you can keep the default or you can enter your own default domain name. It is recommended to create a new host later.

 

DOMAIN=localhost

 

docker-compose.yml configuration

 

The docker-compose.yml template file is the core of using Docker Compose , and it involves more instruction keywords.

 

If you have students who need to learn, you can view the document: Compose template file

 

Here are some examples of OLStack modifications:

 

One, mount the directory

 

    volumes:

        -./config/lsws/conf:/usr/local/lsws/conf

        -./config/lsws/admin-conf:/usr/local/lsws/admin/conf

        -./bin/container:/usr/local/bin

        -./sites:/var/www/vhosts/

        -./acme:/root/.acme.sh/

        -./logs/lsws/:/usr/local/lsws/logs/

 

: The former is the corresponding directory of the host machine (this server), and the relative path is used here. After: is the directory corresponding to the container host, if there are other directory mounting requirements, you can modify volumes: to mount it.

 

Two, open ports **

 

    ports:

      -80:80

      -443:443

      -443:443/udp

      -7080: 7080

 

There are three TCP ports open here : 80 ( HTTP ), 443 ( HTTPS ) and 7080 ( OLS backend).

 

There is also a UDP port: 443 ( QUIC , HTTP/3 )

 

If there are more needs, you can add new ports.

 

For security, you can modify -7080:7080 to a more secure port, such as: -27080:7080 such non-default ports to reduce the possibility of security attacks.

 

3. Start the function with #

 

The default green # with the function is not enabled:

 

# phpmyadmin:

# image: phpmyadmin/phpmyadmin:latest

# container_name: phpmyadmin

# ports:

#-"8081:80"

# environment:

#-PMA_HOST=mysql

#-PMA_PORT=3306

#-TZ=${TimeZone}

 

Like phpmyadmin , phpredisadmin , memcached are currently disabled by default. If necessary, remove the # at the top , and then re-run the container orchestration.

 

::: warning Warning adminer , phpmyadmin , phpredisadmin are recommended to be closed when not in use. :::

Directory Structure

LiteSpeed container

 

    volumes:

        -./config/lsws/conf:/usr/local/lsws/conf ## OLS configuration file directory

        -./config/lsws/admin-conf:/usr/local/lsws/admin/conf ## OLS management console directory

        -./bin/container:/usr/local/bin ## Related tool files

        -./sites:/var/www/vhosts/ ## Virtual host storage location

        -./acme:/root/.acme.sh/ ## The storage address of the certificate generated by Let's Encrypt

        -./logs/lsws/:/usr/local/lsws/logs/ ##OLS log address

 

The most important thing is - ./sites:/var/www/vhosts/ and - ./logs/lsws/:/usr/local/lsws/logs/

 

Here is a relative path. If the OLStack directory is in /home/webserver/OLStack, then the actual path of ./sites is /home/webserver/OLStack/OLStack .

 

If there are servers with additional data disks, it is recommended to put the OLStack directory on the data disk to run.

MySQL container

 

    volumes:

      -"./data/mysql:/var/lib/mysql:delegated"

 

./data/mysql is the directory where the physical files of the database are stored.

 

Students who need to customize my.cnf can modify the docker-compose.yml file to mount the corresponding file.

Redis-Server container

 

    volumes:

      -./config/redis/redis.conf:/etc/redis.conf

      -./data/redis/data:/data

      -./logs/redis/:/var/log/redis/

 

-./config/redis/redis.conf:/etc/redis.conf configuration file, with Chinese comments

 

-./data/redis/data:/data persistent physical file directory

 

-./logs/redis/:/var/log/redis/ Redis-Server log directory

Instructions for use

 

::: warning prompt to use the following command, first you have to enter the OLStack directory :::

Change LiteSpeed ​​WebAdmin password

 

bash bin/webadmin.sh <your_password>

 

For example, if I want to change to 123456, then enter:

 

bash bin/webadmin.sh 123456

 

Create virtual host

 

bash bin/domain.sh -add <your_domain.com>

 

For example, if I want to create a virtual host with a domain name of mf8.biz , then enter it, with www. No need to repeat the input:

 

bash bin/domain.sh -add mf8.biz

 

Delete virtual host

 

domain.sh -del <your_domain.com>

 

Create database

 

The following command will automatically generate the user name, password and database name. Automatically generated using the following:

 

bash bin/database.sh -domain <your_domain.com>

 

Use the following method to customize the user name, password and database name, replacing user_name , my_password and database_name with the desired values:

 

bash bin/database.sh -domain <your_domain.com> -user user_name -password my_password -database database_name

 

Connect to the database

 

Normally use the database. In the scenario where the site database is not separated, the general database connection address is filled in: 127.0.0.1 or localhost .

 

In the Docker environment, because the database and the execution language are run separately, they are not in the same "server", so the local connection address cannot be used naturally. We need to use mysql instead.

Instructions for use

Configure SSL certificate

 

The SSL certificate is implemented by ACME applying for a Let's Encrypt free certificate, and ACME needs to be installed for the first run .

Install ACME

 

Only need to install ACME for the first run, run with email notification:

 

./bin/acme.sh --install -email <EMAIL_ADDR>

 

E.g:

 

./bin/acme.sh --install -email cert@mf8.biz

 

No email notification is required to run:

 

./bin/acme.sh --install --no-email

 

Apply for a certificate

 

Use this command in the root domain, you do not need to fill out www. Will automatically add www. :

 

./bin/acme.sh -domain <yourdomain.com>

 

E.g:

 

./bin/acme.sh -domain mf8.biz

 

Two certificates www.mf8.biz and mf8.biz will be automatically issued

Update OLS version

 

To upgrade OpenLiteSpeed to the latest stable version, please run

 

bash bin/webadmin.sh -lsup

 

Install WAF firewall

 

Use ModSecurity to implement the firewall WAF function:

 

Enable WAF on the web server :

 

bash bin/webadmin.sh -modsec enable

 

Disable WAF on the web server :

 

bash bin/webadmin.sh -modsec disable

 

phpMyAdmin

 

address:

 

http://yourip:8080

 

http://yourip:8443

 

The default username is root , and the password is the same as the password you provided in the .env file.

Go inside the container

 

docker exec -it litespeed / bin / sh # enter OpenLiteSpeed , PHP container

docker exec -it mysql / bin / bash # into the MariaDB / Percona Server container

docker exec -it redis / bin / sh # enter Redis Server container

 

As long as the container name is defined: container_name , then replace <container_name> with the name of the container name to enter.

 

docker exec -it <container_name> /bin/sh

 

Container tutorial

Docker tutorial

 

Front docker run back - name litespeed in litespeed is $ name , which represents the identifier of the container, i.e. $ name = litespeed .

 

1. Define the name variable, which can also be modified to mysql , redis, etc.

 

name=litespeed

 

2. Check the online status and size of the container

 

docker ps -as

 

Three, view the running output log of the container

 

docker logs $name

 

4. Restart the container, generally use it after modifying the configuration other than the port to make the modification effective

 

docker restart $name

 

Five, stop the operation of the container

 

docker stop $name

 

Six, remove the container

 

docker rm $name

 

Seven, view the docker container occupies CPU , memory and other information

 

docker stats --no-stream

 

Docker-Compose tutorial

 

::: warning prompt first have to enter there docker-compose.yml template files directory. :::

 

docker-compose up ## Start all containers temporarily

docker-compose up -d ## Persistently start all containers

docker-compose stop ## Stop container operation

docker-compose down ## Stop and delete all containers

 

If you modify the docker-compose.yml file, you need to rebuild.

 

docker-compose up --build

 

Other links

 

LLStack OLStack Community Edition Container Image

 

Rice cake
