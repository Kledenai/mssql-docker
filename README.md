# Microsoft Sql Server

A mssql database service to run on docker

## running the application without docker compose

We will be using `docker run` to be able to run **getting-started**, before running the service we will need create the volume and network to be used for mssql, this is only in the case for you use the `docker run` in the `docker compose` you won't need create a volume and network before run a service, follow the command below to create a volume:

```bash
docker volume create mssql
```

After running the volume creation command, run the command below to view the created volume:

```bash
docker volume ls
```

This is the expected return:

```bash
DRIVER    VOLUME NAME
local     mssql
```

Having successfully created the local volume, follow the command bellow to create a network:

```bash
docker network create mssql --driver bridge
```

Note that I am using one option in creating the network, below are the option and what each of them do:

- **driver**: Driver to manage the Network

After running the network creation command, run the command below to view the created network:

```bash
docker network ls
```

This is the expected return:

```bash
NETWORK ID     NAME       DRIVER    SCOPE
4eba0f206c3a   mssql      bridge    local
```

Everything working out we can go to the next step, follow the line below to able to run the postgres service:

```bash
docker run -dp 1433:1433 -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=yourStrong(!)Password" -e "MSSQL_PID=Express" --volume mssql:/var/opt/mssql --network mssql  mcr.microsoft.com/mssql/server:2019-latest
```

After the services running you can run the command below to check the health of the container:

```bash
docker ps
```

This is the expected return:

```bash
CONTAINER ID   IMAGE                                        COMMAND                  CREATED             STATUS            PORTS                                        NAMES
53681c1282f7   mcr.microsoft.com/mssql/server:2019-latest   "/opt/mssql/bin/perm…"   4 minutes ago       Up 4 minutes      0.0.0.0:1433->1433/tcp, :::1433->1433/tcp    hand_peaceful            
```

Having had this return with this result, let's access the service, using the management software to access the sql server in `localhost:1433`

## running the application with docker compose

We will use docker-compose to be able to run the script already prepared to be able to run the service in docker, follow the example below of what is expected to be in the file:

```bash
version: '3.9'

services:
  mssql:
    image: mcr.microsoft.com/mssql/server:2019-latest
    environment:
      ACCEPT_EULA: "Y"
      SA_PASSWORD: "${SA_PASSWORD}"
      MSSQL_PID: "Express"
    ports:
      - 1433:1433
    volumes:
      - mssql:/var/opt/mssql
    networks:
      - mssql

volumes:
  mssql:

networks:
  mssql:
    driver: bridge
```

Note that variable references are being passed, which are being taken from a `.env` file, you will need to have an `.env` file in your project root containing the same parameters below:

```bash
SA_PASWORD=
```

Add the values ​​and then save the file, having done that we can run the file.

Only this is enough for us to run the service, if everything is ok, run the command below:

```bash
docker-compose up -d
```

After run the command you can run the command below to check the health of the containers:

```bash
docker ps
```

This is the expected return:

```bash
CONTAINER ID   IMAGE                                        COMMAND                  CREATED             STATUS            PORTS                                        NAMES
53681c1282f7   mcr.microsoft.com/mssql/server:2019-latest   "/opt/mssql/bin/perm…"   4 minutes ago       Up 4 minutes      0.0.0.0:1433->1433/tcp, :::1433->1433/tcp    hand_peaceful 
```

Having had this return with this result, let's access the service, using the management software to access the sql server in `localhost:1433`.

Everything being in agreement you have already managed to run the microsoft sql server on docker.
