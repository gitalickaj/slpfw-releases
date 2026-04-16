# SLPFW — Security Lockdown Pro Firewall

Enterprise-grade Linux security daemon with **90 security modules** across 9 phases. Zero-dependency static binary. Defense-in-depth with kernel-level eBPF/XDP filtering, nftables firewall, AI-based threat prediction, and post-quantum cryptography.

[![Go 1.25+](https://img.shields.io/badge/Go-1.25+-00ADD8?logo=go)](https://go.dev)
[![Linux](https://img.shields.io/badge/OS-Linux-FCC624?logo=linux)](https://kernel.org)
[![Release](https://img.shields.io/github/v/release/gitalickaj/slpfw-releases)](https://github.com/gitalickaj/slpfw-releases/releases/latest)

---

## Quick Install

```bash
# Download latest release
curl -sLO https://github.com/gitalickaj/slpfw-releases/releases/latest/download/install.sh
sudo bash install.sh
```

The installer automatically detects your OS, installs dependencies, configures nftables/eBPF, generates TLS certificates, and starts the service.

### After Installation

```
Web Panel:  https://<server-ip>:9080/
Login:      admin / admin  (change on first login!)
CLI:        slpfw-cli status
Logs:       journalctl -u slpfw -f
```

### Install Options

```bash
sudo bash install.sh --ssh-port 22 --password MySecurePass
sudo bash install.sh --no-interactive --force
```

| Option | Description |
|--------|-------------|
| `--ssh-port PORT` | SSH port (default: auto-detect) |
| `--password PASS` | Web panel password |
| `--license KEY` | License key |
| `--no-interactive` | No prompts |
| `--force` | Overwrite existing config |

---

## Features

### 90 Security Modules in 9 Phases

| Phase | Category | Modules |
|-------|----------|---------|
| 1 | **Core** | Engine, Event Bus, Config, Logger, Self-Defense, Watchdog (3 levels), SQLite Storage |
| 2 | **Firewall & Network** | nftables, eBPF/XDP, DDoS, Port Scan, ICMP, ARP, IPv6, DNS Sinkhole, Egress, Socket Monitor |
| 3 | **Access Control** | LFD (Login Failure), Account Monitor, Sudo Audit, SSH Key Monitor, Credential Stuffing, Lateral Movement, Zero Trust |
| 4 | **Web Protection** | WAF (XSS/SQLi/RCE), HTTP Rate Limiter, Bot Detection, API Shield, JA3 Fingerprint, RASP, Honeypot |
| 5 | **System** | Rootkit Detector, Crypto Miner Detector, File Integrity, Malware Scanner, Fileless Attack, Scheduler Monitor |
| 6 | **Data & Services** | DB Guard, Log Protector, Backup Integrity, Mail Guard, Container Monitor |
| 7 | **Intelligence** | Threat Intel Feeds, GeoIP, Behavioral Profiling, AI Prediction, Threat Score, Anonymizer Detection |
| 8 | **Crypto & Response** | TLS Enforce, Post-Quantum (ML-KEM), Log Signing (Ed25519), Guardian Auto-Recovery, Emergency Lockdown, Forensic Audit |
| 9 | **Boot & Ops** | Boot Integrity, Kernel Guard, Secure Boot, USB Guard, Auto-Update, CLI (50+ commands), REST API, Web Dashboard |

### Key Capabilities

- **Wire-speed filtering** via eBPF/XDP (kernel bypass)
- **Stateful firewall** with nftables dynamic rules
- **Multi-vector DDoS mitigation** (SYN, UDP, DNS amplification)
- **AI threat prediction** with behavioral profiling (Welford's algorithm)
- **Post-quantum cryptography** (ML-KEM key exchange via Cloudflare CIRCL)
- **Self-defense**: seccomp BPF, capability drop, ptrace protection, binary signing
- **Three-level watchdog**: userspace goroutine, systemd timer, kernel module (kprobe)
- **Multi-server management** via mTLS central dashboard
- **14 languages** with CLDR plural forms and RTL support
- **Zero CGo dependencies** — single static binary

---

## Architecture

```
Incoming Packet
      │
      ▼
┌─────────────┐
│  eBPF/XDP   │ ← Wire-speed filtering (kernel bypass)
└──────┬──────┘
       │ PASS
       ▼
┌─────────────┐
│  nftables   │ ← Stateful firewall rules
└──────┬──────┘
       │ PASS
       ▼
┌─────────────┐
│  Rate Limit │ ← Per-IP/subnet rate limiting
└──────┬──────┘
       │ PASS
       ▼
┌─────────────┐
│  WAF Engine │ ← XSS, SQLi, RCE detection
└──────┬──────┘
       │ PASS
       ▼
   Application
```

---

## CLI Usage

```bash
slpfw-cli status                    # Show all module status
slpfw-cli deny 1.2.3.4 -r "spam"   # Block an IP
slpfw-cli allow 10.0.0.5            # Whitelist an IP
slpfw-cli list banned               # List all banned IPs
slpfw-cli flush                     # Flush temporary bans
slpfw-cli score 203.0.113.50        # Get threat score
slpfw-cli geo block CN              # Block country
slpfw-cli emergency-lockdown        # Full system lockdown
```

---

## Requirements

- **OS**: Linux (kernel 5.10+ recommended)
- **Arch**: amd64 (x86_64)
- **Privileges**: Root
- **systemd**: Required for service management

---

## Uninstall

```bash
slpfw-uninstall --purge --force
```

---

## Auto-Update

SLPFW updates itself automatically from GitHub Releases. Enable in the web panel under Settings → Auto-Update.

---

## Documentation

Full documentation is available at the web panel: `https://<server-ip>:9080/docs`

---

## License

Proprietary. All rights reserved.

Copyright (c) 2026 AEU — Elton Aliçkaj
