Create ec2 instance

ssh -i your-key.pem ubuntu@YOUR_PUBLIC_IP

sudo apt update && sudo apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Enable Docker
sudo systemctl enable docker
sudo systemctl start docker

# Add ubuntu user to docker group
sudo usermod -aG docker $USER
newgrp docker

# Install Docker Compose
sudo apt install -y docker-compose

ubuntu@ip-172-31-41-181:~$
docker --version


output will be : Docker version 29.2.1, build a5c7197

docker compose version

output will be docker Compose version v5.1.0

git clone https://github.com/harathi-mutyam/docker-compose-react-nodejs-mysql.git

ls

cd docker-compose-react-nodejs-mysql

ls

vim .env

replace EC2_PUBLIC_IP=51.20.4.150 with your ec2-instance-public-ip
and also replace at #CLIENT_ORIGIN=http://YOUR_PUBLIC_IP

CLIENT_ORIGIN=http://51.20.4.150
CLIENT_API_BASE_URL=http://51.20.4.150:3500/api
 in the above lines also
 
:wq!   #save the file

ubuntu@ip-172-31-39-109:~/docker-compose-react-nodejs-mysql$ vim docker-compose.yml


services:
  mysqldb:
    image: mysql:5.7
    restart: unless-stopped
    env_file: ./.env
    environment:
      - MYSQL_ROOT_PASSWORD=$MYSQLDB_ROOT_PASSWORD
      - MYSQL_DATABASE=$MYSQLDB_DATABASE
    ports:
      #- "3306:3306"       # mapped to standard MySQL port
       - "${MYSQLDB_LOCAL_PORT}:${MYSQLDB_DOCKER_PORT}"
    volumes:
      - db:/var/lib/mysql
    networks:
      - backend

  bezkoder-api:
    depends_on:
      - mysqldb
    build: ./bezkoder-api
    restart: unless-stopped
    env_file: ./.env
    ports:
      #- "3500:8080"       # backend accessible on 3500
       - "${NODE_LOCAL_PORT}:${NODE_DOCKER_PORT}"
    environment:
      - DB_HOST=mysqldb
      - DB_USER=$MYSQLDB_USER
      - DB_PASSWORD=$MYSQLDB_ROOT_PASSWORD
      - DB_NAME=$MYSQLDB_DATABASE
      - DB_PORT=3306
      - CLIENT_ORIGIN=$CLIENT_ORIGIN
    networks:
      - backend
      - frontend

  bezkoder-ui:
    depends_on:
      - bezkoder-api
    build:
      context: ./bezkoder-ui
      args:
        - REACT_APP_API_BASE_URL=$CLIENT_API_BASE_URL
    ports:
      #- "80:80"           # frontend accessible on port 80
       - "${REACT_LOCAL_PORT}:${REACT_DOCKER_PORT}"
    networks:
      - frontend

volumes:
  db:

networks:
  backend:
  frontend:



check this one also same content is available in docker-compose.yml file after git clone 
docker compose up --build -d

check it in browser with 
ec2-publicip:80
try to add books

if not added down the docker compose once again up it

docker compose down

docker compose up --build -d

check it again in internet explorer
 it will work 

 at the end 

 docker compose down
 
 


