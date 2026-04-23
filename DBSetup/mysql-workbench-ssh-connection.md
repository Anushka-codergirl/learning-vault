# MySQL Workbench Configuration Using SSH (OCI Instance)

## 🔐 Step 1: Convert .ppk → .pem
Use PuTTYgen:
- Open PuTTYgen
- Click Load → select your `.ppk file`
- Go to Conversions → Export OpenSSH key
- Save as `key.pem`

---

## 🖥️ Step 2: Create Connection in MySQL Workbench
Open Workbench → + (New Connection)

**Connection Method:** Standard TCP/IP over SSH

### SSH Section:
- **SSH Hostname:** `your-oci-public-ip:22`
- **SSH Username:** `username` (check vm instance image for reference)
- **SSH Key File:** Select your `key.pem`


### MySQL Section:
- **MySQL Hostname:** `127.0.0.1`
- **MySQL Server Port:** `3306`
- **Username:** `mysql_username`
- **Password:** Store in vault

--- 

## ⚙️ Step 3: Important OCI Server Setup
On VM (via SSH), ensure MySQL allows connections:
- Check MySQL is running:
```bash
sudo systemctl status mysql
```
- Allow remote
```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
- find: 
```bash
bind-address = 127.0.0.1
```
Keep it as-is if using SSH tunnel ✅
(secure method)


--- 

## 🔥 Step 4: Open OCI Ports (if needed)
Go to OCI → VCN → Security List:
Allow: `Port 22 (SSH)`

---

## ✅ Step 5: Test Connection
Click:
👉 Test Connection

If correct → it will connect via SSH tunnel 🎉