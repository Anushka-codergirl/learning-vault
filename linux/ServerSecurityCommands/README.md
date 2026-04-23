# 🛡️ Server Security Quick Commands

A quick reference guide for managing firewall rules, monitoring suspicious activity, and maintaining Nginx-based WordPress servers.

---

## 🧱 Firewall (iptables)

**List all active DROP rules:**
```bash
sudo iptables -L INPUT -v -n | grep DROP
```

**Block specific IP:**
```bash
sudo iptables -A INPUT -s <IP> -j DROP
```

**Unblock IP:**
```bash
sudo iptables -D INPUT -s <IP> -j DROP
```

**Block multiple IPs at once:**
```bash
for ip in 1.2.3.4 5.6.7.8; do
  sudo iptables -A INPUT -s $ip -j DROP
done
```

**Save firewall rules (persist after reboot):**
```bash
sudo netfilter-persistent save
sudo netfilter-persistent reload
```

**View saved rules file:**
```bash
sudo cat /etc/iptables/rules.v4
```

---

## 🔍 Logs & Monitoring

**Check recent Nginx access logs:**
```bash
sudo tail -n 50 /var/log/nginx/access.log
```

**Watch live log (real-time):**
```bash
sudo tail -f /var/log/nginx/access.log
```

**Find most frequent IPs hitting your site:**
```bash
sudo awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head
```

**Find top endpoints accessed:**
```bash
sudo awk '{print $7}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head
```

**Find who accessed WooCommerce API:**
```bash
sudo grep "wp-json/wc/store" /var/log/nginx/access.log | awk '{print $1}' | sort | uniq -c | sort -nr | head
```

**Filter logs for checkout or cart spam:**
```bash
sudo grep "POST /wp-json/wc/store" /var/log/nginx/access.log
```

---

## 🌐 Nginx Control

**Test config before reload:**
```bash
sudo nginx -t
```

**Reload after config change (no downtime):**
```bash
sudo systemctl reload nginx
```

**Restart Nginx completely:**
```bash
sudo systemctl restart nginx
```

**Check Nginx status:**
```bash
sudo systemctl status nginx
```

---

## ⚙️ Fail2Ban (Optional)

**Check active bans:**
```bash
sudo fail2ban-client status
```

**Check details for Nginx jail:**
```bash
sudo fail2ban-client status nginx-http-auth
```

**Unban an IP:**
```bash
sudo fail2ban-client set nginx-http-auth unbanip <IP>
```

---

## 💡 Useful Tools & Checks

**Check open ports:**
```bash
sudo netstat -tulpn | grep LISTEN
```

**Lookup suspicious IP info:**
```bash
whois <IP>
```

**Block IP range (CIDR):**
```bash
sudo iptables -A INPUT -s <IP_RANGE>/24 -j DROP
```

**View system journal (for errors or security events):**
```bash
sudo journalctl -xe
```

---

## 🧰 Tips

- Always **test Nginx config** before reloading to avoid downtime.
- Keep a backup of your `/etc/nginx/sites-available/` and `/etc/iptables/rules.v4`.
- Regularly monitor `access.log` for repeated hits on `/wp-json/`, `/xmlrpc.php`, `/cart`, `/checkout`, or `/wp-login.php`.

---
