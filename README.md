- [TIGStack](#tigstack)
  - [Requirements](#requirements)
  - [Grafana configuration](#grafana-configuration)
  - [InfluxDB configuration](#influxdb-configuration)
  - [Telegraf configuration](#telegraf-configuration)
    - [Generate additional Telegraf configuration](#generate-additional-telegraf-configuration)


# TIGStack

Telegraf InfluxDB Grafana is probably the best for monitoring !

## Requirements

- `docker-compose.yml` file is written with version 3.9, please make sure that your will support this version
- **Please**, modify .env file, especially passwords for Grafana and InfluxDB

## Grafana configuration

>A bunch of settings can be configured via environment variables, feel free to add additional settings

Grafana use a custom multi-stage build to perform a self-signed certificate and provide HTTPS support for the web ui, on port 443.

Default credentials (**please edit .env file**) :  
- username : `admin`
- password : `s3cret`

## InfluxDB configuration
>A bunch of settings can be configured via environment variables, feel free to add additional settings

InfluxDB use a custom multi-stage build to perform a self-signed certificate and provide HTTPS support on port 8086.  
To enforce security, InfluxDB have many users with different level of permissions (**please edit .env file**) : 

|    Username   |    Password   |  Permissions |
|:-------------:|:-------------:|:------------:|
| admin         | s3cret        | full access  |
| influx_writer | s3cret_writer | write access |
| influx_reader | s3cret_reader | read access  |


## Telegraf configuration

Telegraf use environment variable to perform the connection to the InfluxDB instance.   
You can refer to the existing `certs.conf ` Telegraf configuration file to use the same syntax

### Generate additional Telegraf configuration

You can use the following example to generate additional configuration files on `telegraf.conf.d` folder : 

`telegraf --output-filter influxdb --input-filter x509_certs config > telegraf.conf.d/certs.conf`  
