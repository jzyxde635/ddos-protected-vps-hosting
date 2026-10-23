# DDoS Protected VPS Hosting: What the 600 Gbps Label Really Means, and How to Pick a Plan That Survives an Actual Flood

"DDoS protected VPS hosting" is one of the least standardized phrases in the hosting market. Two providers can print the same three words on a plan card while doing completely opposite things when a flood arrives: one filters the attack upstream and keeps your site online, the other withdraws your IP from the routing table and waits for the attacker to get bored.

So the useful question is not "how many Gbps do you advertise." It's what happens at the edge of that number. With GoMami — the provider this article is centered on — the honest answer is more specific than its own homepage suggests: default protection is best-effort and free, the headline 600 Gbps scrubbing tier is a paid add-on, and if an attack gets big enough to threaten neighbouring customers, your IP gets blackholed for 30 to 60 minutes. That's not a dealbreaker, but it's the kind of detail you want before you pay, not at 3 a.m.

**Quick pick:** most people shopping for a China-optimised box with real mitigation capacity should look at the Hong Kong Pulse line first — HKG.Pulse.Mini at $59/month gets 2 vCPU, 4 GB RAM, 60 GB NVMe, 1 TB outbound and a 1 Gbps port. If single-core speed matters (game servers, databases), HKG.Turin.Mini at $69/month runs on an EPYC 9575F and is worth the extra $10. 👉 [Compare the Hong Kong Pulse plans and current stock](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-pulse)

## Scrubbing and blackholing are both sold as "protection"

Volumetric attacks have to be stopped above your server, because by the time your kernel sees the packets, the pipe is already full. Nobody fixes a 40 Gbps UDP flood with iptables. That leaves two mechanisms, and they protect different parties.

Scrubbing pulls traffic into a filtering layer upstream, drops the attack packets there, and lets legitimate traffic continue to your box. Blackholing — null-routing, RTBH — removes your IP from the routing table entirely, so nothing reaches it. The attack stops. So do your users, your monitoring, and your SSH session.

Blackholes exist for a reason: absorbing a large flood costs transit money, and a provider that null-routes one customer to keep the rest of a subnet online is making a defensible call. The tell is in the wording. "Protection up to X Gbps" is a ceiling, and the sentence after it about what happens past the limit is usually the null-route clause. "Protected IP available as an add-on" means the default address isn't protected. GoMami's documentation is unusually explicit about all of this, which is why it's worth reading closely rather than skimming the homepage.

## What GoMami's DDoS protection actually includes

Every GoMami instance ships with best-effort basic scrubbing: automatic, free, no configuration, no switch to flip. The 600 Gbps professional scrubbing service is a separate paid tier, and GoMami's docs say to contact support or check the website for details — no public price is listed.

The documented behaviour worth knowing:

- **Default tier:** automatic detection and basic scrubbing, capacity varies with attack size and network conditions. GoMami does not publish a number for this tier.
- **Advanced tier:** 600 Gbps scrubbing, paid add-on.
- **Blackhole policy:** when a large attack affects other customers on the same network segment, the affected IP is null-routed for **30–60 minutes**, released automatically, with **no manual intervention possible**. All inbound and outbound traffic to that IP is dropped during the window.
- **Covered attack types (documented):** L3/L4 — UDP flood, SYN flood, ACK flood, ICMP flood, DNS amplification, NTP amplification. L7 — HTTP flood, CC attacks, Slowloris, with the caveat that L7 policies vary by node and protection tier.

That 30–60 minute window is the most useful number on this page. Some hosts review null-routes on a six-hour cycle, which can mean most of an evening offline after a five-minute attack. GoMami's auto-release is comparatively fast. It's still a blackhole, though, so if your workload is a game server where a 30-second outage triggers a player disconnect wave, treat the default tier as a floor rather than a solution. 👉 [Check the current Hong Kong plans where this protection applies](https://gomami.io/aff.php?aid=123&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-turin)

Correction on that link — the correct one is here: 👉 [See HKG Turin configurations and pricing](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-turin)

## The full GoMami lineup, line by line

GoMami sells four locations and two hardware platforms, plus a dedicated-server line. All VPS plans include the "China Mainland Optimized Pro" routing — CN2 for Telecom, AS9929 for Unicom, CMIN2 for Mobile — with premium return paths on the Asian nodes and dual-direction premium routing in Los Angeles.

### Hong Kong — HKG Turin (AMD EPYC 9575F, Zen 5, up to 5.0 GHz, PCIe Gen5 + DDR5)

| Plan | vCPU / RAM / NVMe | Traffic | Port | Price/mo | Order |
| --- | --- | --- | --- | --- | --- |
| Turin.Mini | 2 / 4 GB / 100 GB | 1 TB | 2 Gbps | $69 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-turin) |
| Turin.Air | 4 / 8 GB / 140 GB | 2 TB | 2 Gbps | $129 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-turin) |
| Turin.Pro | 6 / 16 GB / 180 GB | 5 TB | 5 Gbps | $299 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-turin) |
| Turin.Ultra | 12 / 32 GB / 220 GB | 10 TB | 5 Gbps | $599 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-turin) |

Turin.Pro and Turin.Ultra are marked Windows-ready. Official latency to mainland China from Hong Kong: under 30 ms.

### Hong Kong — HKG Pulse (AMD EPYC 7763, up to 3.5 GHz)

| Plan | vCPU / RAM / NVMe | Traffic | Port | Price/mo | Order |
| --- | --- | --- | --- | --- | --- |
| Pulse.Nano | 2 / 2 GB / 40 GB | 500 GB | 1 Gbps | $49 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-pulse) |
| Pulse.Mini | 2 / 4 GB / 60 GB | 1 TB | 1 Gbps | $59 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-pulse) |
| Pulse.Air | 4 / 8 GB / 80 GB | 2 TB | 1 Gbps | $119 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-pulse) |
| Pulse.Pro | 8 / 16 GB / 100 GB | 5 TB | 3 Gbps | $269 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-pulse) |
| Pulse.Ultra | 12 / 32 GB / 300 GB | 10 TB | 3 Gbps | $499 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-pulse) |

Pulse.Pro and Pulse.Ultra are Windows-ready.

### Japan — JPN Pulse (Tokyo, EPYC 7763 class)

| Plan | vCPU / RAM / NVMe | Traffic | Port | Price/mo | Order |
| --- | --- | --- | --- | --- | --- |
| Pulse.Nano | 2 / 2 GB / 40 GB | 500 GB | 1 Gbps | $29 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |
| Pulse.Mini | 2 / 4 GB / 60 GB | 1 TB | 1.5 Gbps | $49 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |
| Pulse.Air | 4 / 8 GB / 80 GB | 2 TB | 1 Gbps | $89 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |
| Pulse.Pro | 8 / 16 GB / 100 GB | 5 TB | 3 Gbps | $169 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |
| Pulse.Ultra | 12 / 32 GB / 300 GB | 10 TB | 3 Gbps | $338 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fjpn-pulse) |

Official latency: under 50 ms to mainland China.

### Singapore — SIN Pulse

| Plan | vCPU / RAM / NVMe | Traffic | Port | Price/mo | Order |
| --- | --- | --- | --- | --- | --- |
| Pulse.Nano | 2 / 2 GB / 40 GB | 500 GB | 1 Gbps | $29 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fsin-pulse) |
| Pulse.Mini | 2 / 4 GB / 60 GB | 1 TB | 1 Gbps | $49 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fsin-pulse) |
| Pulse.Air | 4 / 8 GB / 80 GB | 2 TB | 1 Gbps | $89 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fsin-pulse) |
| Pulse.Pro | 8 / 16 GB / 100 GB | 5 TB | 3 Gbps | $169 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fsin-pulse) |
| Pulse.Ultra | 12 / 32 GB / 300 GB | 10 TB | 3 Gbps | $338 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fsin-pulse) |

Official latency: under 70 ms.

### Los Angeles — LAX Pulse (EPYC 7K62, 3.3 GHz, dual-direction CN2 GIA / AS9929 / CMIN2)

| Plan | vCPU / RAM / NVMe | Traffic | Port | Price/mo | Order |
| --- | --- | --- | --- | --- | --- |
| Pulse.Nano | 2 / 2 GB / 40 GB | 1 TB | 1 Gbps | $29 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Flax-pulse) |
| Pulse.Mini | 2 / 4 GB / 60 GB | 2 TB | 1 Gbps | $59 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Flax-pulse) |
| Pulse.Air | 4 / 8 GB / 80 GB | 4 TB | 2 Gbps | $129 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Flax-pulse) |
| Pulse.Pro | 6 / 16 GB / 100 GB | 8 TB | 3 Gbps | $259 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Flax-pulse) |
| Pulse.Ultra | 12 / 32 GB / 300 GB | 15 TB | 5 Gbps | $599 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Flax-pulse) |
| Pulse.Titan | 12 / 32 GB / 600 GB | 30 TB | 10 Gbps | $999 | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Flax-pulse) |

LAX gives the most traffic per dollar of any line. GoMami publishes no latency figure for Los Angeles, so don't take a number from a blog post as gospel — test it yourself inside the refund window.

### Hong Kong — HKG Forge (dedicated server, EPYC 7663, 56C/112T, TYAN B8033)

| Plan | RAM / NVMe | Traffic | Port | Price/mo | Order |
| --- | --- | --- | --- | --- | --- |
| Forge.Mini | 128 GB / 960 GB | 10 TB | 2 Gbps | $599 + $68 setup | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-forge) |
| Forge.Air | 256 GB / 4 TB | 20 TB | 2 Gbps | $899 + $68 setup | [Order](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore%2Fhkg-forge) |

Forge traffic overage is billed at $0.06/GB, unlike the VPS lines. Extra IPs are $10 each, maximum four per server.

### Also on the homepage: the Ryzen 9 9950X line

GoMami's front page features three Ryzen 9 9950X plans (up to 5.7 GHz) that aren't part of the four product lines above: Mini 2 vCPU / 4 GB / 40 GB / 1 TB / 2 Gbps at $59, Air 4 / 8 GB / 60 GB / 2 TB / 2 Gbps at $99, and Pro 6 / 16 GB / 80 GB / 5 TB / 5 Gbps at $199. Third-party roundups list different prices for what looks like the same hardware, so treat the homepage numbers as the reference and confirm at checkout. 👉 [Browse the GoMami store to see which lines are live](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore)

## How the rest of the market sells the same two words

Useful calibration, because "DDoS protected" pricing models differ wildly. All figures below are from the providers' own published documentation.

| Provider | What the included protection covers | What happens past the limit |
| --- | --- | --- |
| Liquid Web | Standard tier free: volumetric attacks up to 2 Gbps | Advanced ($99/mo per server, up to 10 Gbps) or Premium ($499/mo per account, full L4) |
| VPSDime | Built-in capacity absorbs smaller attacks | Target IP null-routed; routes reviewed every 6 hours, so recovery can take hours |
| BuyVM | Filtered IPs sold separately at $3/month per IP | Unfiltered addresses are null-routed while an attack is in progress |
| GoMami | Best-effort scrubbing free on every instance, no configuration | IP blackholed 30–60 minutes, released automatically; 600 Gbps scrubbing available as a paid add-on |

Read that column on the right twice. GoMami's free tier is weaker than a paid scrubbing subscription, but its blackhole window is far shorter than a six-hour review cycle. If you're choosing between providers on mitigation alone, ask each one the same question: at what threshold do you null-route me, and for how long?

## The billing rules that change your real annual cost

Three things shape the actual bill more than the sticker price.

**Only outbound traffic is metered.** Inbound is free, which matters if you run mirrors, backups, or anything that pulls data in volume.

**Overshooting your traffic allowance throttles you rather than billing you.** VPS plans drop to 20 kb/s until the next billing cycle — the box stays up, just barely usable. Forge dedicated servers are the exception at $0.06/GB. A 20 kb/s ceiling is effectively downtime for a production site, so size the plan around your real monthly egress instead of hoping.

**Annual billing saves 15–20%, depending on the product line.** Run the arithmetic before choosing a cycle: HKG.Pulse.Mini at $59/month is $708 a year at monthly billing, or roughly $566–$602 on an annual cycle at the stated discount. Turin.Mini at $69/month is $828 a year versus about $662–$704 annually. On a $599 Forge.Mini, the same percentage is a few hundred dollars.

Two policy details that get skipped in most reviews: the refund window is **24 hours from activation**, full stop — nothing after that, and no refunds on renewal orders. Service is also **suspended on the due date with no grace period**, and terminated five days later with data possibly unrecoverable. If you're testing routing, do it in the first day. Set the renewal reminder earlier than you think you need to.

Payment methods are cards via Stripe, Alipay via Stripe, and crypto. Deployment is automatic, usually within one to five minutes, with credentials emailed. OS templates cover Ubuntu 22.04/24.04, Debian 11/12, CentOS Stream 9, AlmaLinux 8/9 and Windows Server 2019/2022, with custom ISO mounting for anything else. Third-party roundups circulate promo codes such as GOMAMI365 and Hi,LAX, but neither appears on an official GoMami page — the cart has a Promo Code field, so validate before paying rather than assuming.

## What an actual attack on a small VPS looks like

Most floods hitting small offshore servers come from booter and stresser subscriptions costing $10–$30 a month, delivering roughly 5–50 Gbps in bursts of a few minutes. That's enough to knock over an unprotected $5 box and nowhere near a 600 Gbps ceiling. The 100–400 Gbps band shows up against gambling, streaming and adult properties; multi-terabit record events exist but you're almost certainly not the target.

The part that catches people out is collateral damage. Many networks blackhole at /24 granularity because that's the smallest prefix most carriers accept for RTBH. If someone else in your subnet gets hit, your IP can go dark with theirs. Capacity headroom upstream is what keeps that decision from ever being made on your behalf — which is the practical argument for the paid scrubbing tier if you're a visible target, and the reason to ask GoMami support what the add-on costs before you commit to a game server or a public API on the default tier.

## What's still your job on the box

Upstream scrubbing protects the pipe. It doesn't tune your kernel, and state-exhaustion attacks slip under volumetric thresholds by design — a few hundred Mbps of SYNs can kill an untuned server while the network graph looks quiet. GoMami's own docs publish three iptables rules as a starting point: rate-limit SYN connections, drop invalid packets, rate-limit ICMP.

Beyond that: enable `tcp_syncookies`, raise `tcp_max_syn_backlog`, size `nf_conntrack_max` to your expected connection count, and use nginx's `limit_req` and `limit_conn` — both cost nothing and stop most non-human L7 floods. Cache whatever can be cached, since the page an attacker wants is always the one you regenerate from the database on every request. Keep SSH and the control panel off the public workload's IP where possible.

One direction people forget: don't become a reflector. An open DNS resolver, an unconfigured NTP daemon, or an exposed memcached turns your box into someone else's amplification source. Inbound floods happen to you; outbound floods are your responsibility, and providers treat them accordingly.

## FAQ

**Is the 600 Gbps protection included by default?** No. GoMami's documentation states that all instances get best-effort basic scrubbing free, and the 600 Gbps professional tier is a paid add-on — contact support for pricing. Several affiliate articles claim otherwise; the docs are the authority.

**Will my IP get blackholed during an attack?** It can be. If an attack affects other customers on the same network segment, the IP is null-routed for 30–60 minutes and released automatically, with no manual override.

**Is inbound traffic counted against my quota?** No. Only outbound traffic is billed.

**What happens if I exceed my monthly traffic?** VPS plans are throttled to 20 kb/s until the next cycle, with no overage charge. Forge dedicated servers are billed $0.06/GB instead.

**Is there a free trial?** GoMami doesn't publish one. The 24-hour refund window is the practical equivalent — deploy, run your own latency and throughput tests against your real users, and cancel within the day if the routing doesn't hold up.

**Do the plans include IPv6?** Store listings show one IPv4 address per instance. Ask support if you need IPv6 before ordering.

## Which plan to buy, by situation

**You want the cheapest way to test GoMami's China routing:** JPN.Pulse.Nano or LAX.Pulse.Nano at $29/month. Two cores, 2 GB, 500 GB or 1 TB of traffic. Use the first day to ping and traceroute from the networks your users actually sit on.

**You're hosting a normal website or API for mainland users:** HKG.Pulse.Mini at $59 or Pulse.Air at $119. The Mini handles most WordPress, Laravel or Django deployments; step up to Air when you stop wanting to think about resource contention.

**Single-thread performance is the constraint — game servers, Redis, InnoDB:** HKG.Turin.Mini at $69 or Turin.Air at $129. The EPYC 9575F runs at up to 5.0 GHz with DDR5 and PCIe Gen5 storage, and the price gap over Pulse is small at the entry level.

**You need one box for Chinese and North American users:** LAX.Pulse.Air at $129 with 4 TB of traffic and dual-direction premium routing on all three carriers.

**You're a visible target and the default tier isn't enough:** ask support about the 600 Gbps scrubbing add-on before you deploy, and budget for a plan where a 30–60 minute blackhole wouldn't end your business.

**You need dedicated silicon:** Forge.Mini at $599 plus $68 setup for 56 cores, 112 threads and 128 GB RAM, with the same China-optimised routing underneath.

Before you check out, confirm two things: the billing cycle you actually want (annual saves 15–20%, but the 24-hour refund only applies to new purchases), and whether the add-on scrubbing tier is priced into your plan or not. Then 👉 [pick your GoMami line and location here](https://gomami.io/aff.php?aff=415&url=https%3A%2F%2Fgomami.io%2Fstore) and test the route within the refund window — that day of measurement is worth more than any benchmark table, including this one.
