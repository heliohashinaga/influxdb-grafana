# influxdb-grafana

## Criar container InfluxDB e Grafana

1. Criar network e volumes

```
$ docker network create monitoring
$ docker volume create grafana-volume
$ docker volume create influxdb-volume
```

2. Configurar o container Influx

```
docker run --rm \
  -e INFLUXDB_DB=monitoria -e INFLUXDB_ADMIN_ENABLED=true \
  -e INFLUXDB_ADMIN_USER=admin \
  -e INFLUXDB_ADMIN_PASSWORD=Z6Nwc50pxGTW09vo9LBLvY \
  -v influxdb-volume:/var/lib/influxdb \
  influxdb /init-influxdb.sh
```

3. Executar docker componse

```
$ docker-compose up -d
```

4. Configurar grafana
Acessar o grafana  
   URL: http://localhost:3000  
   User: admin  
   Password: admin

O Grafana irá pedir para trocar a senha depois de logado.
