---
title: "Monitoring your Linux Server with Prometheus"
datePublished: Wed Jan 31 2024 12:00:38 GMT+0000 (Coordinated Universal Time)
cuid: cls1qkdgn000008lafo2db05b
slug: monitoring-your-linux-server-with-prometheus
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689350974196/552f7526-f8fd-487e-ab65-b0f53906eccd.png
tags: prometheus, network-performance-monitoring, system-admin, remote-server-monitoring

---

Every system administrator would be wise to keep a close eye on the health and performance of their server infrastructure. Prometheus, an open-source monitoring and alerting toolkit, empowers system administrators to gain deep insights into their Linux servers, ensuring optimal performance and availability. In this article, I'll walk you through the step-by-step process of installing Prometheus, configuring it to monitor various metrics, adding authentication for secure access, and incorporating exporters to enhance its monitoring capabilities.

## **Installation**

Go to [https://prometheus.io/download/](https://prometheus.io/download/).

In the **Operating System** dropdown menu, choose **linux**.

In the **Architecture** dropdown menu, choose **amd64**.

In the **prometheus** section, find the version that is labeled **LTS**.

Copy the listed URL. It will be labeled something like **prometheus-2.37.6.linux-amd64.tar.gz**.

In your terminal, download the file at the copied URL.

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.37.6/prometheus-2.37.6.linux-amd64.tar.gz
```

> The URL in the line above is just an example, use the actual URL you copied earlier.

The file will be downloaded to your machine. Unzip it.

```bash
tar xvfz prometheus-2.37.6.linux-amd64.tar.gz
```

To stay organized, remove the compressed file as it's no longer needed.

```bash
rm prometheus-2.37.6.linux-amd64.tar.gz
```

Move all of the Prometheus files to `/opt/prometheus`.

```bash
sudo mv prometheus-2.37.6.linux-amd64 /opt/prometheus
```

Create a new user that will run the Prometheus daemon.

```bash
sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
```

Set the new user as the owner of `/opt/prometheus`.

```bash
sudo chown -R prometheus:prometheus /opt/prometheus
```

Create a new systemd service file called `/etc/systemd/system/prometheus.service`.

```yaml
[Unit]
Description=Prometheus Monitoring
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
WorkingDirectory=/opt/prometheus
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml
ReadWriteDirectories=/opt/prometheus

[Install]
WantedBy=multi-user.target
```

Reload systemd daemons.

```bash
sudo systemctl daemon-reload
```

Start the Prometheus service.

```bash
sudo systemctl start prometheus
```

Enable the Prometheus service so it starts when the system starts.

```bash
sudo systemctl enable prometheus
```

Verify it works by going to http://\[your server's IP address/hostname\]:9090.

## **Add Authentication**

Right now, anyone who visits the URL will see all monitoring information. This could leak sensitive data. Prometheus has built-in basic authentication available for configuration.

First, a password hash must be created. You can use Python for this. Debian Linux comes preinstalled with Python 3.

Install `python3-bcrypt`.

```bash
sudo apt update
sudo apt install -y python3-bcrypt
```

Create a file called [`gen-pass.py`](http://gen-pass.py).

```python
import getpass
import bcrypt

password = getpass.getpass("password: ")
hashed_password = bcrypt.hashpw(password.encode("utf-8"), bcrypt.gensalt())
print(hashed_password.decode())
```

Run the file with Python.

```bash
python3 gen-pass.py
```

You will be prompted for a password. Enter a strong and unique password that you will use to log in to Prometheus's web user interface, then press **Enter**.

The password hash is output. Copy it.

Create a file called `/opt/prometheus/web.yml`.

```yaml
basic_auth_users:
    [username]: [copied password hash]
```

For the username, choose any username you like. For the password hash, paste the password hash you copied earlier.

Edit `/etc/systemd/system/prometheus.service` to change the `ExecStart` line.

```bash
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml --web.config.file=/opt/prometheus/web.yml
```

Reload systemd daemons.

```bash
sudo systemctl daemon-reload
```

Restart the Prometheus service.

```bash
sudo systemctl restart prometheus
```

Now, when you access Prometheus in your web browser, a username/password prompt will appear. Enter the username and password you chose earlier to gain access.

## **Adding an Exporter**

Prometheus comes with some basic metrics to monitor by default, but you'll want to use "exporters" which provide access to even more metrics. There are metrics for...

* Your hardware and OS: the "Node exporter" `node_exporter`
    
* MySQL/MariaDB: the "MySQL Server Exporter" `mysqld_exporter`
    
* Many, many more
    

Create a directory to store the exporters.

```bash
sudo mkdir /opt/prometheus/exporters
```

Go to [https://prometheus.io/download/](https://prometheus.io/download/).

In the **Operating System** dropdown menu, choose **linux**.

In the **Architecture** dropdown menu, choose **amd64**.

Find the exporter you want to install. In this example, we'll go with the Node exporter.

In the **node\_exporter** section, copy the listed URL. It will be labeled something like **node\_exporter-1.5.0.linux-amd64.tar.gz**.

In your terminal, download the file at the copied URL.

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
```

Unzip the file.

```bash
tar xvfz node_exporter-1.5.0.linux-amd64.tar.gz
```

Remove the compressed file.

```bash
rm node_exporter-1.5.0.linux-amd64.tar.gz
```

Move the exporter executable to the exporters directory.

```bash
sudo mv node_exporter-1.5.0.linux-amd64/node_exporter /opt/prometheus/exporters
```

Remove the remaining files.

```bash
rm -rf node_exporter-1.5.0.linux-amd64
```

The binary you just copied is still owned by your user account. Change the ownership of it (and all other files in `/opt/prometheus` for good measure) to the `prometheus` user you created earlier.

```bash
sudo chown -R prometheus:prometheus /opt/prometheus
```

Create a new systemd service file at `/etc/systemd/system/prometheus_node_exporter.service`.

```yaml
[Unit]
Description=Prometheus Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/opt/prometheus/exporters/node_exporter

[Install]
WantedBy=multi-user.target
```

Reload systemd daemons

```bash
sudo systemctl daemon-reload
```

Start the exporter

```bash
sudo systemctl start prometheus_node_exporter
```

Enable the exporter so it starts when the system starts.

```bash
sudo systemctl enable prometheus_node_exporter
```

Edit `/opt/prometheus/prometheus.yml`.

Indented under the `scrape_configs` section, add the following.

```yaml
- job_name: node
  static_configs:
  - targets: ['localhost:9100']
```

Restart the Prometheus service.

```bash
sudo systemctl restart prometheus
```

Verify it works by going to `http://[your server IP address or hostname:9090/graph?g0.expr=rate(node_disk_io_time_seconds_total[1m])`. That page will show you the rate of I/O operations of your system disks.

Repeat the process to add as many exporters as you need to fulfill your server monitoring needs.

Implementing Prometheus as your Linux server monitoring solution equips you with powerful tools to keep your infrastructure in check. By following the steps outlined in this guide, you can effectively set up Prometheus, configure it to monitor essential metrics, strengthen security through authentication, and expand monitoring capabilities with exporters. With Prometheus in place, you'll have the ability to proactively identify and address issues, optimize performance, and ensure the stability and reliability of your Linux server environment.

Cover photo by [Safwan Thottoli](https://unsplash.com/@safwan_thottoli?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/3PGJ17syNfQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).