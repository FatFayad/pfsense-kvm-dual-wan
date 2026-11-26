# Troubleshooting notes

Three network bugs I hit while wiring up the dual-WAN pfSense VM.

## 1. PPPoE link dropped

ISP 1 went offline. `ip -br link` and `ethtool` showed the physical NIC and Linux bridge were still UP, so I skipped reseating cables and checked `/var/log/ppp.log` inside pfSense. The log showed an authentication rejection from the ISP side, so it was an ISP session/credential issue and not a local bridge or VM drop.

## 2. Secondary WAN had link but passed zero traffic

The second WAN showed carrier UP inside pfSense, yet nothing got past the upstream router. The upstream router had a MAC whitelist enabled, and KVM had handed the VM interface a generated bridge MAC that wasn't on the list. Matching the VM interface MAC to the whitelisted address fixed it right away.

## 3. Slow browsing even with normal speedtests

Pages felt sluggish even though bandwidth tests and gateway pings looked fine. One ISP's DHCP lease was pushing a slow DNS resolver into pfSense. Turning off ISP DHCP DNS override and setting explicit resolvers cleared the lag.
