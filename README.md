Cred:
https://github.com/bcremer/docker-telegraf-influx-grafana-stack
https://github.com/sruon/telegraf-stocks

# Example Docker Compose project for Telegraf, InfluxDB and Grafana

This an example project to show the TIG (Telegraf, InfluxDB and Grafana) stack.

## Start the stack with docker compose

```bash
$ docker-compose up
```

## Services and Ports

### Grafana
- URL: http://localhost:3000 
- User: admin 
- Password: admin 



### Telegraf
- Port: 8125 UDP (StatsD input)

### InfluxDB
- Port: 8086 (HTTP API)
- User: admin 
- Password: admin 
- Database: influx


Run the influx client:

```bash
$ docker-compose exec influxdb influx -execute 'SHOW DATABASES'
```

Run the influx interactive console:

```bash
$ docker-compose exec influxdb influx

Connected to http://localhost:8086 version 1.8.0
InfluxDB shell version: 1.8.0
>
```

[Import data from a file with -import](https://docs.influxdata.com/influxdb/v1.8/tools/shell/#import-data-from-a-file-with-import)

```bash
$ docker-compose exec -w /imports influxdb influx -import -path=data.txt -precision=s
```

# Usage
```
usage: telegraf_stocks [-h] --ticker TICKER

Retrieves stock information from Yahoo Finance and formats them in InfluxDB
line format.

optional arguments:
  -h, --help            show this help message and exit
  --ticker TICKER       Ticker symbol to check on Yahoo Finance.
```

# Retrieve AAPL stock informations
```
$> python3 ./plugin/telegraf_stocks --ticker BB
```

# Integrate with Telegraf
```
[[inputs.exec]]
  commands = [
    "python3 ./telegraf_stocks --ticker AAPL",
    "python3 ./telegraf_stocks --ticker AMZN",
    "python3 ./telegraf_stocks --ticker GME",
    "python3 ./telegraf_stocks --ticker BB"]

  ## Timeout for command to complete.
  timeout = "10s"

  interval = "1m"

  # Data format to consume.
  # NOTE json only reads numerical measurements, strings and booleans are ignored.
  # data_format = "influx"
  tag_keys = ["symbol"]
  json_string_fields = ["shortName", "symbol"]
  data_format = "json"
```

# Yahoo API throttling
2,000 requests/hr per IP when *unauthenticated*
