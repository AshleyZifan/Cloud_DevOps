# Introduction

Github Url: https://github.com/AshleyZifan/Trading_App

This application is an online stock trading simulation REST API wich allows users to execute market orders. Front end developers, mobile developers and traders can utilize this trading system. It is a MicroService which is implemented with Java SpringBoot. It uses IEX CLOUD API to get market data. and use JDBC connection to store data into PostgresSQL database.

# Docker Architecture Diagram

### trading_app docker diagram 

![Docker](C:\Bluespanda\Repositories\Cloud\Docker.jpg)

### Two docker files

- trading-app 
- jrvs-psql

Use `schema.sql` to create tables.

```
#you may or may not need sudo for docker cmds

#start docker
sudo systemctl start docker
#17.05 or higher
sudo docker -v

#create network bridge between SpringBoot app and postgreSQL
sudo docker network create --driver bridge trading-net

#build trading app
sudo docker build -t trading-app .

#build psql image
cd psql/
sudo docker build -t jrvs-psql .

#run a psql container
sudo docker run --rm --name jrvs-psql \
-e POSTGRES_PASSWORD=password \
-e POSTGRES_DB=jrvstrading \
-e POSTGRES_USER=postgres \
--network trading-net \
-d -p 5432:5432 jrvs-psql

#Setup IEX token
IEX_TOKEN='your_IEX_token'
#run a trading_app container
sudo docker run \
-e "PSQL_URL=jdbc:postgresql://jrvs-psql:5432/jrvstrading" \
-e "PSQL_USER=postgres" \
-e "PSQL_PASSWORD=password" \
-e "IEX_PUB_TOKEN=${IEX_TOKEN}" \
--network trading-net \
-p 5000:5000 -t trading-app

#verify health
curl localhost:5000/health
```

# Cloud Architecture Diagram

### trading app diagram  

![Cloud](C:\Bluespanda\Repositories\Cloud\Cloud.jpg)

# Elastic Beanstalk and Jenkins CI/CD pipeline



