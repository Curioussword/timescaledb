# timescaledb
create your own database for timescaledb integration and postgress - DETERMINE IF YOU ARE GOING LOCAL ON SAME MACHINE OR GOING REMOTE HERE ARE THE STEPS.
________________________________________________________________________________________________________
using ubuntu 22.04
Commands you need to run to install DOCKER below:

sudo apt update

sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
_______________________________________________________________________________________________________
Install DOCKER - 
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
_______________________________________________________________________________________________________

run docker version check - /////    docker --version

sudo systemctl start docker
sudo systemctl enable docker

Docker image after installation to setup Timescale db

docker run -d --name timescaledb -p 5432:5432 -e POSTGRES_PASSWORD=your_password timescale/timescaledb:latest-pg14

check status of running instance:
***
sudo systemctl status docker
***
____________________________________________________________________________________________________________
couple of key things you can check if you do not have the info 
you can use this to check users

\du

You can log into the default database to list all databases: will ask you for password
psql -U postgres -d postgres -h localhost -p 5432
can use \l     ----list all databases

can go ahead and create your database here 

CREATE DATABASE trading_data;

example 

postgres=# CREATE DATABASE trading_data;
CREATE DATABASE
postgres=# \l
                                  List of databases
     Name     |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
--------------+----------+----------+------------+------------+-----------------------
 postgres     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0    | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
              |          |          |            |            | postgres=CTc/postgres
 template1    | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
              |          |          |            |            | postgres=CTc/postgres
 trading_data | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 


_______________________________________________________________________________________________________________

*** can access the container's shell - 

docker inspect examplename


docker exec -it timescaledb bash


apt-get update
apt-get install nano  --- if needed or use vi

for me my volume is in this directory - cd /var/lib/postgresql/data -- you need to check your volume by using this command for your setup/environment inside of the container. use this command     docker inspect examplename

now... nano postgresql.conf ----ensure in this file that limit_address is set to '*' for remote access. can exit this file

After, nano pg_hba.conf and use 

host    all             all             remote_ip_address/32          md5

add this line at the bottom of the pg_hba.conf file



create python file - For Database Connection Function - Create a reusable function to connect to TimescaleDB:

make sure to save and exit.
You may run into trouble with firewall related things if you have external configs set and will need to adjust those.

Last step and your on your own creating your database. You want to be on your local host -----here is command

***************
*****************************
tip here default user is postgress if it was not set during docker image creation - and yes you connect locally that your docker image is running on
psql -U your_user -d your_db -h localhost -p 5432
mine for example  ---    psql -U postgres -d trading_data -h localhost -p 5432
*******************************  again you can use this command to check your environment or setup ----    docker inspect examplename
*************  
Now we can create a table and for my example usage I am storing ohlcv data

CREATE TABLE trading_data (
    timestamp TIMESTAMPTZ NOT NULL PRIMARY KEY,
    open NUMERIC,
    high NUMERIC,
    low NUMERIC,
    close NUMERIC,
    volume NUMERIC,
    ema_20 NUMERIC,
    ema_200 NUMERIC
);

SELECT create_hypertable('trading_data', 'timestamp');


**** To use your database and you are connecting remotely this is something you will need to install 

sudo apt update
sudo apt install libpq-dev python3-dev

then you can pip install psycopg2

_____________________________________________
also can use this to view your docker image
*****
docker ps
*****

This is all for now














