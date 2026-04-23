# Deploying Spring Boot Application on Linux

This guide explains how to deploy a Spring Boot application on a Linux server. It covers building the application, transferring the JAR file, running it as a service without using existing JDKs.

---

## Create or use an OCI Ubuntu compute instance

## Check what Java versions are already installed
```bash
java -version
sudo alternatives --display java
```
Let us confirm our current default Java and the installed Java paths. 

### Install JDK 17 or higher version
```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
```

### Copy JAR
From local machine to host machine using **winscp**.

Create a working directory for Spring Boot Applications
```bash
sudo mkdir -p /opt/springboot/apps
sudo mv /home/ubuntu/app-0.0.1-SNAPSHOT.jar /opt/springboot/apps/app-0.0.1-SNAPSHOT.jar
sudo chown -R ubuntu:ubuntu /opt/springboot/apps
```

### Check JDK Path
```bash
ls /usr/lib/jvm
/usr/lib/jvm/java-17-openjdk-amd64/bin/java -version
```
If the path exists, run Spring Boot Application directly with it:
```bash
/usr/lib/jvm/java-17-openjdk-amd64/bin/java -jar /opt/springboot/apps/app-0.0.1-SNAPSHOT.jar --server.port=8080
```

### For a permanent service, create:
```bash
sudo nano /etc/systemd/system/myapi.service
```
Paste this:
```ini
[Unit]
Description=Spring Boot APP
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/opt/springboot/apps
Environment="JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64"
ExecStart=/usr/lib/jvm/java-17-openjdk-amd64/bin/java -jar /opt/springboot/apps/app-0.0.1-SNAPSHOT.jar --server.port=8080
Restart=always
RestartSec=10
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target
```
Save and exit. Then enable:
### Reload, start, check status & view logs
```bash
sudo systemctl daemon-reload
sudo systemctl enable myapi
sudo systemctl start myapi
sudo systemctl status myapi
journalctl -u myapi -f
```

---

### Local Testing
```bash
# On the VM, confirm app is running
curl http://localhost:8080
```

### If service fails
Check the last logs:
```bash
journalctl -u myapi -n 100 --no-pager
```

## Open the network port in OCI
To access from browser, allow port 8080.
**OCI Security List/Networking Security Group**
```
OCI Console → Networking → Virtual Cloud Networks
VCN → Security Lists (or Network Security Groups)
Add Ingress Rules:

Source CIDR: 0.0.0.0/0
IP Protocol: TCP
Destination Port Range: 8080
```
Save changes

#### Open Port in the VM's Firewall (using iptables)
```bash
sudo iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
sudo netfilter-persistent save
```

#### Test It
```bash
# From browser
http://<your-vm-public-ip>:8080
```

---

## Useful Commands

- Restart app: `sudo systemctl restart springboot-app`
- Stop app: `sudo systemctl stop springboot-app`
- Check logs: `journalctl -u springboot-app -f`
- Switch between java versions (globally) - `sudo update-alternatives --config java`
Output example:
```bash
There are 4 choices for the alternative java:

  Selection    Path                                      Priority
------------------------------------------------------------
* 0            /usr/lib/jvm/java-17-openjdk-amd64/bin/java   auto mode
  1            /usr/lib/jvm/java-8-openjdk-amd64/bin/java
  2            /usr/lib/jvm/java-11-openjdk-amd64/bin/java
  3            /usr/lib/jvm/java-17-openjdk-amd64/bin/java
  4            /usr/lib/jvm/java-21-openjdk-amd64/bin/java
```
Select the number you want