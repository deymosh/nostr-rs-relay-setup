# Nostr Relay - Docker Compose Edition

A **production-ready** dockerized deployment of [nostr-rs-relay](https://github.com/scsibug/nostr-rs-relay) with Umbrel web interface and Caddy reverse proxy included.

## 🎯 Features

- **Fully functional Nostr relay** based on nostr-rs-relay
- **Intuitive web interface** with Umbrel for monitoring and management
- **Automatic HTTPS** with Let's Encrypt via Caddy
- **Instant setup** - only 3 steps to get started
- **Fully dockerized** - no external dependencies
- **Persistent database** with SQLite
- **Automatic reverse proxy** for multiple domains

## 📋 Requirements

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- Your own domain (optional - works with dynamic domains like `NoIP`)
- Ports 80 and 443 accessible from the internet

## 🚀 Quick Setup

### Step 0: Clone and initialize submodules

```bash
git clone https://github.com/deymosh/nostr-rs-relay-setup
cd nostr-rs-relay-setup
git submodule update --init --recursive
```

### Step 1: Configure the Caddyfile

```bash
cp Caddyfile.default Caddyfile
```

Edit `Caddyfile` and replace the values:

```caddy
{
    email your@email.com  # Email for Let's Encrypt certificates
}

# Replace these domains with yours
your.domain.com {
    reverse_proxy public-relay:8080
}

dashboard.your.domain.com {
    reverse_proxy public-relay-web:3000
}
```

### Step 2: Configure the Relay

```bash
cp nostr-rs-relay/config.toml config.toml
```

Edit `config.toml` and customize:

```toml
[info]
relay_url = "wss://your.domain.com/"
name = "My Nostr Relay"
description = "A fast and reliable Nostr relay"
pubkey = "your-pubkey-hex"
contact = "mailto:your@email.com"

[network]
address = "0.0.0.0"
port = 8080
```

### Step 3: Start the services

```bash
docker compose up
```

Done! Your relay will be available at:
- **Relay WebSocket:** `wss://your.domain.com`
- **Web Dashboard:** `https://dashboard.your.domain.com`

## 📦 Service Structure

| Service | Function | Internal Port | Access |
|---------|----------|-----------------|--------|
| **public-relay** | Nostr relay (nostr-rs-relay) | 8080 | Through Caddy |
| **public-relay-web** | Umbrel Dashboard | 3000 | Through Caddy |
| **caddy** | Reverse proxy + HTTPS | 80, 443 | Public |

## 📁 Important Files

- `config.toml` - Relay configuration (copy from nostr-rs-relay)
- `Caddyfile` - Reverse proxy configuration (customize)
- `docker-compose.yaml` - Service orchestration
- `relay_data/` - Persistent database (SQLite)
- `caddy_data/` - SSL certificates and Caddy data

## 🔧 Customization

### Enable administrative contact

In `config.toml`:

```toml
pubkey = "your-pubkey-hex"
contact = "mailto:your@email.com"
```

### Add custom icon

Place a `favicon.ico` file in the root and uncomment in `config.toml`:

```toml
favicon = "favicon.ico"
```

## 🐳 Useful Commands

```bash
# View logs in real-time
docker compose logs -f public-relay

# View only relay logs
docker compose logs -f public-relay

# Restart a service
docker compose restart public-relay

# Stop all services
docker compose down

# Stop and remove persistent data (⚠️ careful)
docker compose down -v
```

## 🔐 Security

- Caddy automatically generates and renews SSL certificates
- HTTPS enforced (automatic HTTP to HTTPS redirection)
- Relay only listens on Docker internal network
- Database stored in encrypted persistent volume


## 📚 References

- [nostr-rs-relay GitHub](https://github.com/scsibug/nostr-rs-relay)
- [Nostr Documentation](https://github.com/nostr-protocol/nostr)
- [Caddy Documentation](https://caddyserver.com/docs/)
- [Umbrel Documentation](https://github.com/getumbrel/umbrel)

## 📝 License

This project inherits the license from the original [nostr-rs-relay](https://github.com/scsibug/nostr-rs-relay) repository.

## 🤝 Contributions

Contributions are welcome. For major changes, please open an issue first to discuss what you would like to change.

## ⚡ Support

For issues specific to nostr-rs-relay, consult the [original repository](https://github.com/scsibug/nostr-rs-relay).

For issues with this Docker Compose configuration, open an issue in this repository.

