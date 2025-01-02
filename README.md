# Grafana, Telegraf, and InfluxDB Docker Stack

This stack sets up a monitoring and logging solution using **Grafana**, **Telegraf**, and **InfluxDB**. Telegraf collects metrics and sends them to InfluxDB, where the data is stored. Grafana is then used to visualize the data from InfluxDB.

## Prerequisites

- **Docker** and **Docker Compose** installed on your system.
- Basic knowledge of monitoring systems, InfluxDB, Telegraf, and Grafana.
- A `.env` file containing the necessary environment variables for InfluxDB.

## Stack Overview

- **InfluxDB**: Time-series database where metrics are stored.
- **Telegraf**: Data collection agent that collects metrics from the host system and sends them to InfluxDB.
- **Grafana**: Data visualization tool that queries InfluxDB to visualize metrics in dashboards.

## Docker Compose Configuration

### 1. Clone the Git repository (or prepare your files)

If you haven't already, clone or create your Docker Compose stack:

```bash
git clone https://github.com/NotBeCursed/TIG.git
cd cd TIG
```

Ensure your directory contains the following structure:

```plaintext
docker-compose.yml
entrypoint.sh
.env
telegraf/
    telegraf.conf
```

### 2. Configure your `.env` file

Create a `.env` file in the root directory to define necessary environment variables for InfluxDB initialization. This file will be loaded into the containers to configure them.

Example `.env` file:

```ini
DOCKER_INFLUXDB_INIT_MODE=setup
DOCKER_INFLUXDB_INIT_USERNAME=myuser
DOCKER_INFLUXDB_INIT_PASSWORD=mypassword
DOCKER_INFLUXDB_INIT_ORG=myorg
DOCKER_INFLUXDB_INIT_BUCKET=mybucket
DOCKER_INFLUXDB_INIT_RETENTION=30d
DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=myadmin-token
DOCKER_INFLUXDB_INIT_HOST=influxdb
DOCKER_INFLUXDB_INIT_PORT=8086
```

Make sure to adjust the configuration as needed, especially if you want to customize what metrics Telegraf collects.

### 3. Start the stack

After configuring the files, you can start the entire stack by running:

```bash
docker-compose up -d
```

This will:

- Start InfluxDB, Telegraf, and Grafana.
- InfluxDB will be initialized with the environment variables provided in `.env`.
- Telegraf will start collecting metrics and sending them to InfluxDB.
- Grafana will be available on port `3000` to visualize the data.

### 4. Access Grafana

Once the containers are up and running, you can access Grafana via `http://localhost:3000` in your web browser.

- **Username**: `admin`
- **Password**: `admin`

You will be prompted to change the password on first login.

### 5. Add InfluxDB as a data source in Grafana

After logging in, go to **Configuration** > **Data Sources** > **Add data source** and select **InfluxDB**. Enter the following details:

- **URL**: `http://influxdb:8086` (use the InfluxDB service name defined in the `docker-compose.yml`)
- **Database**: `telegraf` (or the database you specified in the `telegraf.conf` file)

### 6. Create Dashboards

You can now create Grafana dashboards to visualize the metrics coming from InfluxDB. Grafana has many pre-built dashboards that you can import, or you can create your own custom dashboards.

## Troubleshooting

- **Container fails to start**: Check logs using `docker-compose logs <container_name>` to troubleshoot.
- **Grafana can't connect to InfluxDB**: Ensure that the `influxdb` container is running and accessible. If using Docker Compose, ensure the network is properly configured.
- **Telegraf isn't sending data**: Check the Telegraf logs (`docker-compose logs telegraf`) for errors related to configuration.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


