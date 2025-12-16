# Satori Network Installer

Pre-configured installation packages for running a Satori Neuron on different platforms.

## Platforms

| Platform | Directory | Status |
|----------|-----------|--------|
| Linux | `linux/` | Ready |
| Windows | `windows/` | Ready |
| macOS | `mac/` | Ready |

## Quick Start

### 1. Download

Clone this repository or download the zip for your platform:

```bash
git clone https://github.com/SatoriNetwork/Installer.git
cd Installer
```

### 2. Choose Your Platform

Navigate to your platform directory:
- `linux/satori/` - Linux (Ubuntu, Debian, etc.)
- `windows/satori/` - Windows 10/11
- `mac/satori/` - macOS

### 3. Follow Platform README

Each platform has its own `readme/README.md` with detailed setup instructions.

## What's Included

Each platform package contains:

```
{platform}/satori/
├── config/
│   └── config.yaml       # Node configuration
├── docker-compose.yaml   # Docker container setup
├── readme/
│   └── README.md         # Platform-specific instructions
├── wallet/               # Wallet files (add your own)
├── data/                 # Data storage
└── models/               # AI model storage
```

## Configuration

### Networking Modes

Edit `config/config.yaml` to set your networking mode:

```yaml
# Options: central (default), hybrid, p2p
networking mode: central
```

| Mode | Description |
|------|-------------|
| `central` | All traffic through central servers (most stable) |
| `hybrid` | P2P with central fallback (recommended for testing) |
| `p2p` | Pure P2P, fully decentralized |

### Oracle Mode (Advanced)

Run as a data oracle to provide observations to the network:

```yaml
oracle:
  enabled: true
  streams:
    - "your-stream-id"
  stake_amount: 1000  # Minimum SATORI stake required
```

### Prediction Settings

Configure commit-reveal for P2P predictions:

```yaml
predictions:
  auto_reveal: true
  commit_buffer_seconds: 60
```

### Reward Settings

Configure reward claiming:

```yaml
rewards:
  auto_claim: true
  claim_threshold: 10
  claim_address: ""  # Override default (uses vault address)
```

## Docker Requirements

All platforms require Docker:
- [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)
- [Docker Desktop for Mac](https://docs.docker.com/desktop/install/mac-install/)
- [Docker Engine for Linux](https://docs.docker.com/engine/install/)

**Important:** Enable host networking in Docker Desktop settings for Windows/Mac.

## Running

```bash
cd {platform}/satori
docker compose up -d
```

## Viewing Logs

```bash
# All logs
docker logs -f satorineuron

# Individual component logs
docker exec -it satorineuron cat neuron.log
docker exec -it satorineuron cat engine.log
docker exec -it satorineuron cat data.log
```

## Stopping

```bash
docker compose down
```

## Architecture

Satori Neuron runs three integrated components in one container:

| Component | Purpose |
|-----------|---------|
| **Neuron** | Network coordination, UI, wallet management |
| **Engine** | AI prediction engine |
| **Data Server** | Data management and storage |

## P2P Network

When running in `hybrid` or `p2p` mode, your node participates in:

- **Peer Discovery** - Find other nodes via DHT
- **Data Sharing** - Receive observations via GossipSub
- **Predictions** - Commit-reveal prediction protocol
- **Rewards** - Decentralized reward distribution

See [satorip2p documentation](https://github.com/SatoriNetwork/satorip2p) for details.

## Ports

| Port | Purpose |
|------|---------|
| 24600 | P2P server (open to network) |
| 24601 | UI (localhost only recommended) |

## Troubleshooting

### Check Container Status
```bash
docker ps
```

### Check Port Usage
```bash
# Linux/Mac
netstat -an | grep 24600

# Windows
netstat -an | findstr 24600
```

### Reset Data
```bash
docker compose down
rm -rf data/* models/*
docker compose up -d
```

## Support

- [GitHub Issues](https://github.com/SatoriNetwork/Installer/issues)
- [Discord](https://discord.gg/satori)

## License

See [LICENSE](LICENSE) file.
