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

Depending on if you choose to go local or remote to store your tables
can create a python file which is what i did 
can make a directory - mkdir
inside of directory - touch example.py
pip install psycopg2-binary - to connect to postgreSQL.

from here i will have the .py files pushed to repository.







