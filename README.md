# pfsense-kvm-dual-wan

My home router is a pfSense VM running under KVM/libvirt on my Debian box. A 4-port NIC is split across four Linux bridges so the host hands two WAN lines (ISP 1 PPPoE fiber and ISP 2 DHCP broadband), the main LAN, and a separate access-point subnet straight into the VM.

Normal traffic load-balances across both ISPs with weights, and latency-sensitive traffic uses a tiered failover gateway group so games and calls stay on the primary line unless it drops.

![Home network topology](network-map.svg)

## KVM host CPU fix

Out of the box the pfSense VM sat around 115% host CPU even at idle because of KVM halt polling. Turning it off dropped host CPU usage to ~16%:

```bash
echo 0 | sudo tee /sys/module/kvm/parameters/halt_poll_ns
```

## Files

- `configs/interfaces.example` - Debian bridge setup for the 4-port NIC
- `docs/troubleshooting-log.md` - three network bugs I hit and traced from logs while setting this up
