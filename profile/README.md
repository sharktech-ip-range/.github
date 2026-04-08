
If you've ever run a firewall rule, built an IP allowlist, or tried to figure out why traffic from a certain subnet keeps showing up in your logs — you already know how important it is to understand who owns which IP space.

Sharktech is one of those providers that comes up a lot. Game servers, VPS deployments, hosting companies, and security researchers all end up asking the same question sooner or later: *what IP ranges does Sharktech use?*

This article breaks it all down — the ASN, the IPv4 blocks, where they're located, and what it actually means for you depending on why you're looking.

---

## Who Is Sharktech?

Founded in 2003 in Las Vegas, Nevada, Sharktech has been quietly running its own network infrastructure for over two decades. They operate as an independent ISP — not a reseller sitting on top of someone else's backbone. They peer at major Internet Exchange Points and run carrier-grade equipment across five data centers: **Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam**.

Their speciality is DDoS protection. Every server and VPS on their network ships with mitigation built-in, up to 60 Gbps on standard plans — scaling to 1 Tbps for enterprise configurations. That's why you see a lot of game server companies and hosting providers nested inside their IP space.

---

## Sharktech's ASN: AS46844

The single most important piece of information for anyone researching Sharktech IP ranges: **their Autonomous System Number is AS46844**.

Every IP block Sharktech routes to the public internet flows through this ASN. There is no secondary ASN — it's the only one associated with their network.

Here's a quick summary of the AS46844 profile:

- **ASN:** AS46844 (SHARKTECH)
- **Registry:** ARIN
- **Allocated:** March 31, 2009
- **Total IPv4 addresses:** ~121,000+
- **IPv4 prefixes originated:** ~401
- **IPv6 prefixes originated:** 9
- **BGP Peers:** 150+
- **Upstreams:** 4 (including major Tier-1 transit providers)
- **Headquarters:** 8560 S. Eastern Ave, Suite 210, Las Vegas, NV 89120
- **Looking Glass:** Available at sharktech.net/looking-glass/

The looking glass tool lets you run live BGP queries against their network — useful if you're doing actual network diagnostics rather than just checking reputation.

---

## Sharktech IPv4 Network Blocks

Based on current BGP data (AS46844), here are the main IPv4 address ranges assigned to or routed through Sharktech:

**Primary large blocks:**

| CIDR Block | Size | Notes |
|---|---|---|
| 204.188.192.0/18 | 16,384 IPs | Core Sharktech block |
| 208.98.0.0/21 | 2,048 IPs | Sharktech infrastructure |
| 104.36.176.0/23 | 512 IPs | STL-LAS-ST (Las Vegas) |

**45.58.x.x /24 Series (customer-routed /24 blocks):**

Sharktech also routes a large number of individual /24 blocks in the `45.58.128.0/17` range. These are typically allocated to specific customers, downstream networks, or Sharktech's own infrastructure. Verified Sharktech-owned /24s in this range include:

`45.58.128.0/24`, `45.58.129.0/24`, `45.58.130.0/24`, `45.58.131.0/24`, `45.58.151.0/24`, `45.58.158.0/24` through `45.58.160.0/24`, `45.58.165.0/24`, `45.58.168.0/24` through `45.58.173.0/24`, `45.58.175.0/24`, `45.58.176.0/24`, and many others in the same /17.

The full prefix list runs to ~400+ individual routes, which is expected for a provider of their scale. Some /24s in this range are allocated to downstream customers or resellers.

**In total, the network hosts roughly 121,000+ IPv4 addresses and nearly 800,000 domains** — a significant footprint for an independent ISP.

---

## Three Scenarios Where Sharktech's IP Range Matters

### Scenario 1: You're Building a Firewall Allowlist or Blocklist

This is probably the most common reason someone looks up Sharktech's IP range. You want to either allow or block traffic from their network.

**If you're allowing:** You're probably running a service that needs to communicate with a Sharktech-hosted server — maybe a game backend, a VPN endpoint, or an application server. The simplest approach is to look up the specific server IP, verify it's in AS46844 via a WHOIS lookup, and allow that /32 specifically rather than blanket-allowing the whole ASN.

**If you're blocking:** Keep in mind that Sharktech's IP space is heavily shared — roughly 800,000 domains ride on their infrastructure. Blocking the full ASN would catch a very wide net of legitimate traffic, not just the source you're targeting.

The best tools for working with AS46844 ranges in bulk are `bgpview.io/asn/46844` for current prefix data, or `ipinfo.io/AS46844` for the summarized range list. Both are updated continuously from live BGP feeds.

👉 [Check Sharktech's network and hosting options](https://portal.sharktech.net/aff.php?aff=1626)

---

### Scenario 2: You're Checking IP Reputation for a Sharktech Address

You got a connection from a Sharktech IP and you're trying to figure out if it's clean.

Here's the context: Sharktech IP space is **hosting provider space**, which means it's expected to show up on some reputation blocklists by default. Many enterprise spam filters and WAFs automatically flag hosting ASNs because they're not residential consumer IPs.

This doesn't mean the traffic is malicious — it just means it's coming from a data center. Game servers, VPN exit nodes, automation bots, and legitimate business applications all look the same to a basic IP reputation tool.

What you actually want to check: Is the specific /24 or /32 on any active threat feeds? Tools like IPQualityScore, AbuseIPDB, or Spamhaus SBL give you a more granular picture than just checking the ASN.

Sharktech's network does route Tor exit nodes and anonymous traffic (flagged by IPinfo), so some IPs in their range will show up in Tor-related blocklists — this is normal for large hosting providers.

---

### Scenario 3: You're Evaluating Sharktech as a Hosting Provider and Want to Know What IP You'll Get

If you're considering renting a server or VPS from Sharktech, here's what you can expect:

All Sharktech services include **at minimum 1 IPv4 address** in their AS46844 space. You can add additional IPv4 addresses at order time. IPv6 is also supported on the network.

The specific IP you get will depend on which data center you choose. Each location has its own IP allocation:

- **Los Angeles:** IPs drawn from LA-allocated blocks in the 45.58.x.x range and core blocks
- **Denver:** Denver-allocated subnet ranges
- **Chicago:** Chicago-allocated subnet ranges
- **Las Vegas:** Includes the 104.36.176.0/23 block among others
- **Amsterdam:** EU-based IP space

One practical note: Sharktech explicitly confirms there is **no "residential IP" classification** on their network. All IPs are datacenter IPs. If your use case requires clean residential-looking IPs, that's not what Sharktech offers — but for server hosting, that's exactly what you want.

👉 [Get started with a Sharktech server](https://portal.sharktech.net/aff.php?aff=1626)

---

## Sharktech Network Architecture: Why Their IP Space Behaves Differently

A couple things make Sharktech's network unusual compared to typical shared hosting providers.

**They're their own ISP.** Unlike most hosting companies that buy transit capacity from carriers, Sharktech peers directly at Internet Exchange Points. Their BGP infrastructure peers with 150+ networks and routes through 4 upstream providers including major Tier-1 carriers. This is why their latency benchmarks consistently show sub-millisecond times to major DNS resolvers — there are fewer hops.

**DDoS scrubbing is in-path.** Their DDoS protection isn't a bolt-on service — it's integrated into the routing layer. Traffic targeting any AS46844 IP passes through their mitigation infrastructure first. This means Sharktech IP addresses can handle sustained volumetric attacks that would knock out addresses at providers without on-path scrubbing.

**RPKI validation is active.** Of their ~401 originated prefixes, ~402 pass RPKI ROA validation (including IPv6). Zero invalid routes. This is significant for security-focused users — it means their BGP announcements are cryptographically verified, reducing the risk of route hijacking affecting your traffic.

---

## Sharktech Hosting Plans: Full Comparison

If you're not just researching Sharktech's IP space but actually evaluating their infrastructure, here's a complete rundown of their current offerings.

### Smart VPS (Proxmox-Based, Multi-Region)

Built on Proxmox with triple redundancy and NVMe storage. The unique feature: you get a resource pool you can split into as many VMs as you want, deployable across any of their five data centers. 60 Gbps DDoS protection included on every plan.

| Plan | CPU Cores | RAM | NVMe Storage | Bandwidth | Monthly Price | Annual Price (50% off) | Order |
|---|---|---|---|---|---|---|---|
| Tiny | 1 core | 2 GB | 40 GB | 4 TB | $7.95/mo | $3.98/mo |  [Order Tiny](https://portal.sharktech.net/aff.php?aff=1626&pid=smart-vps) |
| Small | 2 cores | 4 GB | 80 GB | 8 TB | ~$15.95/mo | ~$7.98/mo |  [Order Small](https://portal.sharktech.net/aff.php?aff=1626&pid=smart-vps) |
| Medium | 4 cores | 8 GB | 160 GB | 16 TB | ~$31.95/mo | ~$15.98/mo |  [Order Medium](https://portal.sharktech.net/aff.php?aff=1626&pid=smart-vps) |
| Large | 8 cores | 16 GB | 320 GB | 50 TB | ~$63.95/mo | ~$31.98/mo |  [Order Large](https://portal.sharktech.net/aff.php?aff=1626&pid=smart-vps) |
| XL | 16 cores | 32 GB | 640 GB | 100 TB | ~$99.95/mo | ~$49.95/mo |  [Order XL](https://portal.sharktech.net/aff.php?aff=1626&pid=smart-vps) |
| Colossal | Custom | 64+ GB | 1 TB+ | 300 TB | ~$239.95/mo | ~$119.95/mo |  [Order Colossal](https://portal.sharktech.net/aff.php?aff=1626&pid=smart-vps) |

*All VPS plans: 10 Gbps port, 1 IPv4 included, multi-region deployment, Xeon Gold CPUs. Quarterly billing = 25% off. Semi-annual = 35% off. Annual = 50% off (auto-applied, no coupon needed).*

---

### Bare-Metal Dedicated Servers (Sample Available Configs)

Full hardware access, 1 Gbps to 40 Gbps ports, DDoS protection included. Custom configurations available on request.

| Config | CPU | RAM | Storage | Network | Monthly Price | Order |
|---|---|---|---|---|---|---|
| Entry (6-bay) | Dual Xeon E5-2695v4 (72 cores) | 64 GB | 6×3.5" SATA + 2TB NVMe | 10 Gbps / 300 TB | $209/mo |  [Order](https://portal.sharktech.net/cart.php?a=add&pid=742&aff=1626) |
| Mid (12-bay) | Dual Xeon E5-2695v4 (72 cores) | 64 GB | 12×3.5" SATA + 2TB NVMe | 10 Gbps / 300 TB | $249/mo |  [Order](https://portal.sharktech.net/cart.php?a=add&pid=743&aff=1626) |
| Large (24-bay) | Dual Xeon E5-2695v4 (72 cores) | 64 GB | 24×3.5" SATA + 2TB NVMe | 10 Gbps / 300 TB | $329/mo |  [Order](https://portal.sharktech.net/aff.php?aff=1626) |
| Performance | Dual Xeon Gold 6148 | 256 GB | 2TB NVMe | 10 Gbps / custom | ~$269/mo |  [Order](https://portal.sharktech.net/aff.php?aff=1626) |

*Free setup on all dedicated servers. Coupon code **Y5YET1Z9EK** = 10% recurring discount on dedicated servers. Amsterdam servers = 20% recurring with same code.*

---

### Public Cloud (OpenStack, Hourly Billing)

Scalable compute, API access, OpenStack-based. Pay by the hour or month. DDoS protection available as add-on.

| Tier | CPU | RAM | SSD Storage | Bandwidth | Starting Monthly | Order |
|---|---|---|---|---|---|---|
| Small | 4–8 cores | 8–16 GB | 100 GB | 1 TB | ~$39/mo |  [Deploy](https://portal.sharktech.net/aff.php?aff=1626) |
| Medium | 8–16 cores | 16–32 GB | 250 GB | 2 TB | ~$99/mo |  [Deploy](https://portal.sharktech.net/aff.php?aff=1626) |
| Large | 16–32 cores | 64 GB | 1 TB | 5 TB | ~$199/mo |  [Deploy](https://portal.sharktech.net/aff.php?aff=1626) |
| Enterprise | 64 cores | 128 GB | 5 TB SSD | 20 TB | $499/mo |  [Deploy](https://portal.sharktech.net/aff.php?aff=1626) |
| Custom | Configurable | Configurable | Configurable | Configurable | Contact sales |  [Contact](https://portal.sharktech.net/aff.php?aff=1626) |

*Coupon code **WHTFALL** = 33% recurring discount on Cloud Virtual Data Center / OpenStack services.*

---

## Current Promotions (2026)

No coupon hunting required for VPS — the annual discount (50% off) applies automatically at checkout when you select yearly billing. For other services:

- **Y5YET1Z9EK** — 10% recurring discount on dedicated servers and Cloud Virtual Servers; 20% off Amsterdam resources specifically
- **WHTFALL** — 33% recurring discount on OpenStack Cloud Virtual Data Center services

These are recurring discounts, not one-time deals. You save every billing cycle, not just the first.

---

## Who Should Actually Use Sharktech

For anyone coming at this from a security or network operations angle — if you're writing firewall rules, setting up geo-filtering, or vetting traffic sources — the key facts are: AS46844, ~121,000 IPv4 addresses, five data center locations across the US and EU, RPKI-validated routes.

For anyone considering Sharktech as an actual hosting provider, they fit a particular profile well: you need real DDoS protection baked into the infrastructure rather than sold as a separate add-on, you're comfortable with unmanaged servers and basic sysadmin work, and you want predictable flat-rate billing rather than the metered surprises that come with hyperscaler pricing.

The Smart VPS starting at $7.95/month (or $3.98/month on annual billing) is a legitimate entry point with enterprise-grade hardware underneath. The dedicated servers starting at $209/month come with free setup and a network that's been running without major incidents since 2003.

👉 [Explore all Sharktech plans and check current availability](https://portal.sharktech.net/aff.php?aff=1626)
