# Criação de um container docker InfluxDB e Grafana

# Pré-requisitos
- Docker

# Passo a passo

1. Criar docker-compose.yml

```
version: "3.8"
services:
  grafana:
    image: grafana/grafana
    container_name: grafana
    restart: always
    ports:
      - 3000:3000
    networks:
      - monitoring
    volumes:
      - grafana-volume:/var/lib/grafana
  influxdb:
    image: influxdb
    container_name: influxdb
    restart: always
    ports:
      - 8086:8086
    networks:
      - monitoring
    volumes:
      - influxdb-volume:/var/lib/influxdb
networks:
  monitoring:
volumes:
  grafana-volume:
    external: true
  influxdb-volume:
    external: true
```

2. Criar network e volumes

```
$ docker network create monitoring
$ docker volume create grafana-volume
$ docker volume create influxdb-volume
```

3. Configurar o container InfluxDB

```
$ docker run --rm \
  -e INFLUXDB_DB=monitoria -e INFLUXDB_ADMIN_ENABLED=true \
  -e INFLUXDB_ADMIN_USER=admin \
  -e INFLUXDB_ADMIN_PASSWORD=supersecretpassword \
  -e INFLUXDB_USER=devuser -e INFLUXDB_USER_PASSWORD=secretpassword \
  -v influxdb-volume:/var/lib/influxdb \
  influxdb /init-influxdb.sh
```

INFLUXDB_DB = criar um banco de dados  
INFLUXDB_USER e INFLUXDB_USER_PASSWORD = usuário e senha usuado para gravar e ler no grafana

4. Iniciar o sistema de monitoramento

```
$ docker-compose up -d
```

5. Configurar Grafana

   1. Acessar o grafana  
      URL: http://localhost:3000  
      User: admin  
      Password: admin  
      _O Grafana irá pedir para trocar a senha depois de logado._

   2. Selecionar Add data source no menu  
      Preencher os campos:  
      Nome: InfluxDB
      URL: http://influxbd:8086 (nome do container)  
      Database: monitoria  
      User: devuser  
      Password: secretpassword

## Fonte

[Get system metrics for 5 min with Docker, Telegraf, Influxdb and Grafana](https://towardsdatascience.com/get-system-metrics-for-5-min-with-docker-telegraf-influxdb-and-grafana-97cfd957f0ac)
