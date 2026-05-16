# 🚀 Mt-UDP

<p align="center">
  <img src="https://img.shields.io/badge/Go-1.24+-00ADD8?style=for-the-badge&logo=go" />
  <img src="https://img.shields.io/badge/QUIC-HTTP%2F3-2563EB?style=for-the-badge&logo=cloudflare" />
  <img src="https://img.shields.io/badge/License-Proprietary-7C3AED?style=for-the-badge" />
</p>

<p align="center">
  <b>High Performance UDP Proxy Server</b><br>
  Built with <b>Hysteria</b>, <b>QUIC/HTTP3</b>, and <b>gRPC</b>
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
- TLS support
- DPI bypass support
- Multi-user authentication

</td>
</tr>

<tr>
<td width="50%">

### 👥 User Management
- Password authentication
- Username/password mode
- Lock & unlock users
- Force disconnect support

</td>
<td width="50%">

### 📊 Monitoring
- Real-time bandwidth stats
- Active stream monitoring
- Online user tracking
- gRPC API integration

</td>
</tr>
</table>

---

# 📖 Introduction

**Mt-UDP** adalah high-performance UDP proxy server berbasis **Hysteria** yang dirancang untuk koneksi cepat, stabil, dan efisien.

Server ini mendukung:

- ⚡ QUIC/HTTP3
- 🔐 Obfuscation
- 👥 Multi-user authentication
- 📊 gRPC monitoring
- 🛡️ Force disconnect management

Cocok digunakan untuk:
- client apk Zivpn, dan sejenisnya.
- VPS tunneling
- Anti-DPI bypass
- Gaming proxy
- High-speed UDP forwarding

---

# 🚀 Quick Start

## 1️⃣ Download Binary

### Linux AMD64

```bash
wget -O mt-udp \
https://github.com/MakhlukTunnel/mt-udp/releases/latest/download/udp-linux-amd64

chmod +x mt-udp
```

### Linux ARM64

```bash
wget -O mt-udp \
https://github.com/MakhlukTunnel/mt-udp/releases/latest/download/udp-linux-arm64

chmod +x mt-udp
```

---

# ⚙️ Basic Configuration

Buat file `config.json`

```json
{
  "listen": ":443",

  "tls": {
    "cert": "/path/to/cert.pem",
    "key": "/path/to/key.pem",
    "sniGuard": "default"
  },

  "obfs": {
    "type": "salamander",
    "salamander": {
      "password": "default"
    }
  },

  "auth": {
    "type": "passwords",
    "passwords": [
      "password1",
      "password2"
    ]
  },

  "trafficStats": {
    "type": "grpc",
    "listen": "127.0.0.1:998"
  }
}
```

---

# ▶️ Run Server

```bash
./mt-udp server -c config.json
```

## Debug Mode

```bash
./mt-udp server -c config.json -l debug
```

## JSON Log Format

```bash
./mt-udp server -c config.json -l info -f json
```

---

# 🔐 Authentication Modes

| Mode | Description | Example |
|------|-------------|----------|
| `passwords` | Password only | `"passwords": ["mt1", "user1"]` |
| `userpass` | Username + password | `"userpass": {"alice":"alice123"}` |

---

# 🛡️ Obfuscation

| Type | Description |
|------|-------------|
| `plain` | No obfuscation |
| `salamander` | Password-based obfuscation |

---

# 🔒 TLS SNI Guard

| Mode | Description |
|------|-------------|
| `default` | Default validation |
| `dns-san` | DNS SAN validation |
| `strict` | Strict validation |
| `disable` | Disable validation |

---

# 🔌 gRPC API

Default gRPC server:

```text
127.0.0.1:998
```

Tersedia 2 service utama:

---

## 📊 TrafficService

| Method | Description |
|--------|-------------|
| `GetStats` | Get TX/RX bandwidth per user |
| `GetOnlineUsers` | Get online users & client IP |
| `GetStreams` | Get active stream details |

---

## 👥 AuthService

| Method | Description |
|--------|-------------|
| `AddUser` | Add new user |
| `RemoveUser` | Remove user + force disconnect |
| `UpdatePassword` | Update password + force disconnect |
| `LockUser` | Lock user + force disconnect |
| `UnlockUser` | Unlock user |
| `GetUserStatus` | Get user status |
| `ListUsers` | List all users |

---

# 🧪 gRPC Examples

## Install grpcurl

```bash
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
```

---

## Get Stats

```bash
grpcurl -plaintext \
-d '{"auth":"mt1"}' \
localhost:998 \
traffic.TrafficService/GetStats
```

## Get Online Users

```bash
grpcurl -plaintext \
-d '{"auth":"mt1"}' \
localhost:998 \
traffic.TrafficService/GetOnlineUsers
```

## Lock User

```bash
grpcurl -plaintext \
-d '{"auth":"mt1"}' \
localhost:998 \
traffic.AuthService/LockUser
```

## Unlock User

```bash
grpcurl -plaintext \
-d '{"auth":"mt1"}' \
localhost:998 \
traffic.AuthService/UnlockUser
```

## Update Password

```bash
grpcurl -plaintext \
-d '{"auth":"mt1","new_password":"newpass123"}' \
localhost:998 \
traffic.AuthService/UpdatePassword
```

## Get User Status

```bash
grpcurl -plaintext \
-d '{"auth":"newpass123"}' \
localhost:998 \
traffic.AuthService/GetUserStatus
```

## Remove User

```bash
grpcurl -plaintext \
-d '{"auth":"mt1"}' \
localhost:998 \
traffic.AuthService/RemoveUser
```

## List All Users

```bash
grpcurl -plaintext \
localhost:998 \
traffic.AuthService/ListUsers
```

# 📄 License

This project uses a **Proprietary License**.

Please ensure you have a valid license before deploying or distributing this software.

---

# 🙏 Credits

- Hysteria
- QUIC-Go
- gRPC-Go

---

<p align="center">
  Made with ❤️ by
  <a href="https://t.me/makhluktunnel">MakhlukTunnel</a>
</p>
