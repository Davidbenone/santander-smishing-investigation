# Methodology and lab architecture

## Scope

Investigate a suspicious Santander-themed SMS destination without entering credentials, interacting with operators or attempting to bypass access controls.

## Laboratory topology

```text
Windows 11 host
  |
  +-- VirtualBox: OPNsense firewall
  |     +-- WAN: NAT (internet)
  |     +-- LAN: 172.20.50.1/24
  |     '-- OPT1: 192.168.56.2/24 (host-only management)
  |
  '-- VirtualBox: Windows 10 Flare_VM
        '-- 172.20.50.114/24 via OPNsense LAN
```

OPNsense was configured to restrict traffic from the analysis LAN toward private/internal networks and firewall management. Management access was separated onto the host-only interface. These controls reduce risk but are **not a guarantee of complete containment**.

## Investigation sequence

1. Record and defang the SMS destination.
2. Perform DNS resolution and record observed results.
3. Inspect HTTP/HTTPS responses in the controlled VM.
4. Document browser security warnings and server errors.
5. Capture traffic on OPNsense LAN interface `em1`.
6. Analyse DNS, TCP, HTTP and TLS records.
7. Separate confirmed observations from hypotheses and limitations.
8. Preserve original evidence privately; publish only reviewed and redacted material.

## Suggested Wireshark display filters

The following filters use the **original IP address** because Wireshark requires literal values. These are executable analysis filters, not clickable links.

```text
ip.addr == 198.52.247.66
dns.qry.name contains "santander"
tcp.port == 80 && ip.addr == 198.52.247.66
tcp.port == 443 && ip.addr == 198.52.247.66
tls.handshake.extensions_server_name contains "santander"
```

The SNI filter may return no matches if that field is not visible in the captured handshake.

## Evidence integrity

Original captures should remain private. Calculate SHA-256 hashes, retain capture timestamps and record collection interface and filters before producing redacted derivatives. Avoid publishing cookies, tokens, phone numbers or unrelated network activity.
