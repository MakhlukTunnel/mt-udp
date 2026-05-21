# 🚀 Mt-UDP

<p align="center">
  <img src="https://img.shields.io/badge/Go-1.24+-00ADD8?style=for-the-badge&logo=go" />
  <img src="https://img.shields.io/badge/QUIC-HTTP%2F3-2563EB?style=for-the-badge&logo=cloudflare" />
  <img src="https://img.shields.io/badge/License-Proprietary-7C3AED?style=for-the-badge" />
</p>

<p align="center">
  <b>High Performance UDP over QUIC Tunnel Server</b><br>
  Built with <b>Hysteria Core</b>, <b>QUIC/HTTP3</b>, and <b>gRPC</b>
</p>

<p align="center">
  Optimized for speed, low latency, and modern censorship bypass.
</p>

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 🚀 Performance
- QUIC/HTTP3 transport
- BBR & Brutal congestion control
- Low latency optimized
- High throughput proxy engine

</td>
<td width="50%">

### 🔒 Security
- Salamander obfuscation
- TLS with SNI check 
- DPI bypass support
- Multi-user authentication

</td>
</tr>
<tr>
<td width="50%">

### 👥 User Management
- Password & Userpass modes
- Expiry date support
- Lock & unlock users
- Force disconnect on changes

</td>
<td width="50%">

### 📊 Monitoring
- Real-time bandwidth stats
- Online user tracking
- Active connection monitoring
- gRPC API integration

</td>
</tr>
</table>

---

## 📖 Introduction

**Mt-UDP** adalah high-performance UDP tunnel server berbasis **QUIC** yang dirancang untuk koneksi cepat, stabil, dan efisien.

Server ini mendukung:

- ⚡ QUIC/HTTP3 transport
- 🔐 Salamander obfuscation
- 👥 Multi-user authentication (passwords / userpass)
- 📊 gRPC monitoring API
- 🛡️ Force disconnect management
- ⏰ Expiry date per user

Cocok digunakan untuk:
- Client APK Zivpn dan sejenisnya
- VPS tunneling
- Anti-DPI bypass
- Gaming proxy
- High-speed UDP forwarding

---

## ⚙️ Configuration

### Basic Configuration (Userpass Mode)

<pre>{
  "listen": ":5667",
  "tls": {
    "cert": "/path/to/cert.pem",
    "key": "/path/to/key.pem",
    "sniCheck": "default"
  },
  "obfs": {
    "type": "salamander",
    "salamander": {
      "password": "zivpn"
    }
  },
  "auth": {
    "type": "userpass",
    "userpass": {
      "username1": {
        "password": "pass123",
        "exp_date": "2025-12-31 23:59:59",
        "status": "active"
      },
      "username2": {
        "password": "pass456",
        "status": "active"
      }
    }
  },
  "trafficStats": {
    "type": "grpc",
    "listen": "127.0.0.1:998"
  }
}</pre>

Passwords Mode

<pre>{
  "auth": {
    "type": "passwords",
    "passwords": [
      {
        "password": "pass123",
        "exp_date": "2025-12-31 23:59:59",
        "status": "active"
      },
      {
        "password": "pass456",
        "status": "active"
      }
    ]
  }
}</pre>

---

▶️ Run Server

<pre>./mt-udp server -c config.json</pre>

Debug Mode

<pre>./mt-udp server -c config.json -l debug</pre>

JSON Log Format

<pre>./mt-udp server -c config.json -l info -f json</pre>

---

🔐 Authentication Modes

Mode Description Example
passwords Password only (auto-generated user IDs) "passwords": [{"password":"mt1"}]
userpass Username + password (case-sensitive) "userpass": {"alice":{"password":"123"}}

Expiry Date Format

<pre>YYYY-MM-DD HH:MM:SS</pre>

Example: 2025-12-31 23:59:59

Status Values

Status Description
active User can connect
locked User blocked from connecting

---

🛡️ Obfuscation

Type Description
plain No obfuscation
salamander Password-based obfuscation (default)

---

🔌 gRPC API

Default gRPC server:

<pre>127.0.0.1:998</pre>

📊 TrafficService

Method Description
GetStats Get TX/RX bandwidth per user
GetOnlineUsers Get online users & client IPs

👥 AuthService

Method Description
AddUser Add new user with expiry date
RemoveUser Remove user + force disconnect
UpdatePassword Update password + force disconnect
UpdateExpiry Update expiry date
LockUser Lock user + force disconnect
UnlockUser Unlock user
GetUsers List users (single or all)

---

🧪 gRPC Examples

Install grpcurl

<pre>go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest</pre>

Get Stats

<pre>grpcurl -plaintext \
  -d '{"auth":"username:password"}' \
  localhost:998 \
  traffic.TrafficService/GetStats</pre>

Get Online Users

<pre>grpcurl -plaintext \
  -d '{"auth":"username:password"}' \
  localhost:998 \
  traffic.TrafficService/GetOnlineUsers</pre>

Add User

<pre>grpcurl -plaintext \
  -d '{"id":"newuser","password":"pass123","exp_date":"2025-12-31 23:59:59"}' \
  localhost:998 \
  traffic.AuthService/AddUser</pre>

Lock User

<pre>grpcurl -plaintext \
  -d '{"auth":"admin:adminpass"}' \
  localhost:998 \
  traffic.AuthService/LockUser</pre>

Unlock User

<pre>grpcurl -plaintext \
  -d '{"auth":"admin:adminpass"}' \
  localhost:998 \
  traffic.AuthService/UnlockUser</pre>

Update Password

<pre>grpcurl -plaintext \
  -d '{"auth":"username:oldpass","password":"newpass"}' \
  localhost:998 \
  traffic.AuthService/UpdatePassword</pre>

Update Expiry

<pre>grpcurl -plaintext \
  -d '{"auth":"username:password","exp_date":"2026-01-01 00:00:00"}' \
  localhost:998 \
  traffic.AuthService/UpdateExpiry</pre>

Get All Users

<pre>grpcurl -plaintext \
  -d '{"auth":"admin:adminpass"}' \
  localhost:998 \
  traffic.AuthService/GetUsers</pre>

Remove User

<pre>grpcurl -plaintext \
  -d '{"auth":"admin:adminpass"}' \
  localhost:998 \
  traffic.AuthService/RemoveUser</pre>

---

🔧 Systemd Service

<pre>cat > /etc/systemd/system/mt-udp.service << 'EOF'
[Unit]
Description=Mt-UDP Server
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/mt-udp server -c /etc/mt-udp/config.json
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable mt-udp
systemctl start mt-udp</pre>

---

📁 Directory Structure

<pre>/etc/mt-udp/
├── config.json          # Main configuration
├── geoip.dat           # GeoIP database (auto-download)
└── geosite.dat         # GeoSite database (auto-download)</pre>

---

🐛 Troubleshooting

<pre># Check logs
journalctl -u mt-udp -f

# Check service status
systemctl status mt-udp</pre>

---

📄 License

This project uses a Proprietary License.

Please ensure you have a valid license before deploying or distributing this software.

---

🙏 Credits

· QUIC-Go
· gRPC-Go

---

<p align="center">
  Made with ❤️ by
  <a href="https://t.me/makhluktunnel">MakhlukTunnel</a>
</p>
