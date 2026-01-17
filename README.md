# Nostr Relay - Docker Compose Edition

A **production-ready** dockerized deployment of [nostr-rs-relay](https://github.com/scsibug/nostr-rs-relay) with a tor hidden service and web dashboard.

## 🎯 Features

- **Fully functional Nostr relay** based on nostr-rs-relay
- **Intuitive web interface** for monitoring and management
- **Tor integration** with Onion hidden service
- **Instant setup** - only 3 steps to get started
- **Fully dockerized** - no external dependencies
- **Persistent database** with SQLite
- **Automatic reverse proxy** for multiple domains

## 📋 Requirements

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- **Tor is integrated by default** (hidden service automatically generated)

## 🧅 Tor Hidden Service (Built-in)

Tor is **automatically included** in this setup with Onion hidden service support enabled:

- **Always enabled** - Tor runs in its own container by default
- **Zero configuration** - Hidden service is automatically generated on first run
- **Access your relay anonymously** via `.onion` address
- **Get your onion address** after the services start:

```bash
# Retrieve your .onion address
docker compose exec nostr-relay-dashboard cat /var/lib/tor/hidden_service/hostname
```

```bash
# Backing up all tor data
docker cp nostr-relay-dashboard:/var/lib/tor ./tor_data
```

**Your relay will be accessible via:**
- 🧅 **Tor (Onion):** `ws://your-address.onion`

## 🚀 Quick Setup

### Step 0: Clone and initialize submodules

```bash
git clone https://github.com/deymosh/nostr-rs-relay-setup
cd nostr-rs-relay-setup
git submodule update --init --recursive
```

### Step 1: Configure the Relay

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

### Step 2: Start the services

```bash
docker compose up
```

Done! Your relay will be available at:
- **Relay WebSocket:** `ws://<pub_key>.onion`
- **Web Dashboard:** `http://<pub_key>.onion`

## 📦 Service Structure

| Service | Function | Internal Port | Access |
|---------|----------|-----------------|--------|
| **nostr-relay** | Nostr relay (nostr-rs-relay) | 8080 | Through Caddy |
| **web-dashboard** | Tor hidden service + Caddy | 9050, 9051 | Internal |

## 📁 Important Files

- `config.toml` - Relay configuration (copy from nostr-rs-relay)
- `docker-compose.yaml` - Service orchestration
- `relay_data/` - Persistent database (SQLite)
- `tor_data/` - For backing up Tor hidden service data (manual backup needed)

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
- HTTPS enforced (automatic HTTP to HTTPS redirection)
- Relay only listens on Docker internal network
- Database stored in encrypted persistent volume


## 📚 References

- [nostr-rs-relay](https://github.com/scsibug/nostr-rs-relay)
- [Nostr Documentation](https://github.com/nostr-protocol/nostr)
- [Caddy Documentation](https://caddyserver.com/docs/)

## 📝 License

This project inherits the license from the original [nostr-rs-relay](https://github.com/scsibug/nostr-rs-relay) repository.

## 🤝 Contributions

Contributions are welcome. For major changes, please open an issue first to discuss what you would like to change.

## ⚡ Support

For issues specific to nostr-rs-relay, consult the [original repository](https://github.com/scsibug/nostr-rs-relay).

For issues with this Docker Compose configuration, open an issue in this repository.

