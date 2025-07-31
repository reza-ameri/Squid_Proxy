# 🐙 Squid Proxy Server Configuration

This repository provides a practical and minimal setup for a Squid proxy server with the following features:

- 🔐 Basic HTTP authentication
- 🌐 Allow access only from specific IP ranges
- 🔒 Deny all unauthorized access
- 📁 Organized configuration with include directives

---

## 📦 File Structure

```bash
/etc/squid/
├── squid.conf              # Main configuration file
├── conf.d/                 # Additional configuration files (optional)
└── password                # htpasswd-style password file for authentication
⚙️ Configuration Overview
✅ Authentication

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/password
auth_param basic realm proxy
acl authenticated proxy_auth REQUIRED
Uses basic_ncsa_auth for validating usernames and passwords.

Users must authenticate before accessing the proxy.

You can create the password file with:

sudo apt install apache2-utils
htpasswd -c /etc/squid/password <username>
🌍 Network Access Control

acl localnet src your-ip-local
http_access allow localhost
http_access allow localnet authenticated
http_access deny all


All other traffic is denied by default.

Localhost (127.0.0.1) is always allowed.

🧩 Modular Configuration

include /etc/squid/conf.d/*
This allows you to keep additional rules or logging settings in separate files.

🌐 Hostname
visible_hostname localhost
Sets the hostname that Squid reports in error pages and logs.

🚀 Usage
Install Squid:

sudo apt update
sudo apt install squid -y
Place your config at /etc/squid/squid.conf.

Create a user:

htpasswd -c /etc/squid/password user
Start and enable Squid:

sudo systemctl restart squid
sudo systemctl enable squid
Configure your client system to use your proxy IP and port (default: 3128).

🔍 Testing
You can test the proxy by running:

curl -x http://your-proxy-ip:3128 -U user http://example.com
🛡️ Security Tips
Use strong passwords.

Change the default port if necessary.

Use firewall rules to restrict incoming connections to trusted IPs only.

Consider using HTTPS proxy with SSL bump if needed.


