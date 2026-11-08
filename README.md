# Private cloud costs: what 1,000 VMs cost over five years, where the ~50% saving comes from, and when public cloud stays cheaper (with a full SpinServers plan table)

A 1,000-VM private cloud comes to roughly **$3.89 million over five years**, against **$7.68 million** for the same estate in public cloud. That is Isvara's cost model, spreadsheet dated July 2026, and it works out to a 49.4% saving, about **$758,000 a year**, with the VMs spread across 23 ESXi hosts. OpenMetal, which sells hosted private clouds, reports a similar headline from the other direction: customers who move workloads off public cloud typically cut those bills by about 50%.

Both figures come from companies that sell private clouds, and they measure different things. Isvara's is a five-year total that includes VMware Cloud Foundation licensing. OpenMetal's is a monthly workload bill with no licensing line. Treat ~50% as the optimistic end of the range, and the rest of this article as the work of finding out where your estate actually lands.

What follows: the per-VM unit price and how commitment discounts move it, the share of each server you pay for but cannot sell, four costs that sit outside the VM price, the VM count where the crossover really happens, and the cases where staying on public cloud is cheaper. SpinServers' KVM VPS line, the cheapest way to stand up a first private-cloud node, is priced in the table below.

**Quick pick:** if you are pricing a first step rather than a 23-host cluster, the 12 vCPU / 12 GB / 160 GB SSD plan at **$62.00/mo** with 10 TB of monthly transfer is the one configuration SpinServers prints a price for on its public KVM page. 👉 [See the current SpinServers KVM VPS configurations](https://bit.ly/Spinservers)

## SpinServers plans and prices

SpinServers' public KVM page lists seven configurations, all with RAID-10 SSD storage, a 1 Gbps network port, one IPv4 and one IPv6 address, and monthly transfer allowances from 4 TB to 20 TB.

| Configuration | RAM | Storage | Bandwidth / port | Price | Order |
| --- | --- | --- | --- | --- | --- |
| Intel 2 vCPU | 2 GB DDR4 | 40 GB SSD | 4 TB / 1 Gbps | Priced on order page | [ Order the 2 vCPU plan](https://bit.ly/Spinservers) |
| Intel 4 vCPU | 4 GB DDR4 | 80 GB SSD | 8 TB / 1 Gbps | Priced on order page | [ Order the 4 vCPU plan](https://bit.ly/Spinservers) |
| 6 vCPU | 8 GB DDR4 | 120 GB SSD | 8 TB / 1 Gbps | Priced on order page | [ Order the 6 vCPU plan](https://bit.ly/Spinservers) |
| 8 vCPU | 8 GB DDR4 | 160 GB SSD | 10 TB / 1 Gbps | Priced on order page | [ Order the 8 vCPU plan](https://bit.ly/Spinservers) |
| 12 vCPU | 12 GB DDR4 | 160 GB SSD | 10 TB / 1 Gbps | **$62.00/mo** | [ Order the 12 vCPU / 12 GB plan](https://bit.ly/Spinservers) |
| Intel Xeon 16 vCPU | 16 GB DDR4 | 320 GB SSD | 15 TB / 1 Gbps | Priced on order page | [ Order the 16 vCPU plan](https://bit.ly/Spinservers) |
| Intel Xeon 32 vCPU | 32 GB | 500 GB SSD | 20 TB / 1 Gbps | Priced on order page | [ Order the 32 vCPU plan](https://bit.ly/Spinservers) |

Two notes on the table. The KVM page prints a price only for the 12 vCPU configuration, so the other rows are left blank rather than filled in with a guess; the order page shows the exact figure for the shape you pick. And port 25 is closed by default on every VPS, with opening it subject to a support ticket and SpinServers' discretion. If mail delivery is part of your plan, that is a step to budget for, not a detail to discover later.

## What one VM costs before you add anything

OpenMetal's comparison table prices a 2 vCPU / 4 GB VM at **$69.31/month** with no commitment. At 500 VMs on a one-year commit the same VM drops to **$51.05** (26.3% off). At 2,000 VMs on a three-year commit it reaches **$35.43** (a further 30.6% off). The table was assembled from data collected in March 2021 and May 2024, so the absolute dollars are dated; the shape is the useful part. Discounts come from term length first, then from volume.

At 1,000 VMs, that table shows $51,050/month in public cloud against $15,725/month for a managed private cloud: a gap of $35,325 a month, or **$423,900 a year**.

Isvara starts from the other end, with one standard VM: 4 vCPU, 16 GB RAM, 0.5 TB of storage, committed a year ahead, averaged across the three hyperscalers at **$2,009/year**. That price splits 34% CPU, 34% RAM, 32% disk, which normalises to $171 per vCPU per year, $43 per GB of RAM per year, and $1,286 per TB of disk per year. You can re-price your own VM shapes against those unit costs before talking to any vendor.

Isvara also publishes what it thinks a private cloud should charge per VM class, which is where the 49% headline gets more honest. Gold (double compute) comes out at **$1,747/year**, 13% below public. Silver (the standard shape) at **$1,035/year**, 48% below. Bronze (half compute) at **$679/year**, 66% below. Aims at 50% across the whole estate only work because the small VMs carry most of the saving.

## The 30.7% of every server you pay for and cannot sell

Physical capacity is not sellable capacity, and this is where first-time private cloud budgets usually break. On Isvara's 128-thread host, **88.74 threads are sellable**, with 30.7% held back for HA, hypervisor overhead and a planning buffer. Memory is worse: 523.5 GB of 768 GB per tier, doubled to 1,047.0 GB across two tiers.

Run the arithmetic and the host fits 44 VMs by CPU or 65 by memory, so it is CPU-bound at **44 VMs**. Multiply by 23 hosts and you get 1,012, which is how the 1,000-VM model closes. If your workloads are memory-heavy rather than CPU-heavy, that ratio flips and the host count changes with it. Budget the haircut before you compare $169,000 per host against anything.

## Four costs the per-VM price leaves out

### Egress

Public cloud bills outbound traffic by the gigabyte, separately from the VM price, and it is routinely the second-largest line on the invoice. OpenMetal's model shows 150 TB/month costing **$9,400** in public cloud against **$563** on a private cloud billed at the 95th percentile: an $8,838 monthly gap, $106,050 a year. At 600 TB the gap is $25,900 a month.

The private-cloud side of that model works from ports, not gigabytes. Each Medium cluster and above includes a 1 Gbps egress port, which at a 70% peak-traffic concentration moves about **155 TB/month**. Additional 1 Gbps ports cost **$375/month**. A lower-confidence summary of OpenMetal's TCO writing claims private egress runs 60–80% below public cloud list rates and puts AWS egress at $0.06–0.09/GB; I could not verify the full article, so use your own contract rates rather than that range.

Whichever direction you go, pull twelve months of egress from your billing console before you model anything. 👉 [Check how much transfer each SpinServers plan includes](https://bit.ly/Spinservers)

### Admin hours

A small cloud takes 10–20 admin hours a month, plus off-hours support. A well-architected large cloud takes **40–80 hours a month for 1,000 VMs**, rising slightly per node added. OpenMetal's own comment on its 100-VM comparison is blunt: a few thousand dollars of monthly saving will not cover self-management costs at that size.

24/7 coverage generally needs **three or more admins**, though a well-built cloud handles common hardware failures through redundancy rather than 3 a.m. pages. Multiply 40–80 hours by your loaded engineer rate before you believe any per-VM comparison. At a fully loaded $60/hour, 40 hours a month is $28,800 a year, and that is the low end.

### Virtualisation licensing

Of Isvara's **$169,000 per host over five years**, **$80,000 is VMware Cloud Foundation licensing**. The remaining $89,000 covers hardware, admin time and data centre costs. Licensing is 47% of the host budget, which is why a private cloud model that quietly omits it produces fantasy numbers.

Search summaries put VCF list pricing near $350 per core per year; I could not verify that against a current price book, so treat it as a pointer and get a quote. If you are licensing per core, note that the sellable-thread haircut does not reduce the licence count.

### The refresh cycle

A private cloud is a stack of **20-plus software components**, each with its own lifecycle, and every update has to be cross-tested against the rest. Nordcloud's 2020 write-up, which leans toward public cloud, makes the capacity point sharply: assume a four-year refresh and years three and four of expansion become very expensive, because you are buying into hardware that is about to be replaced. Extending maintenance into year five has its own price.

That article is six years old and its author sells public cloud migration, but the mechanics have not changed. Add a refresh line to the five-year model and the saving narrows.

## The VM count where the crossover sits

OpenMetal's own tipping column stays empty at 100 VMs, reads **"Maybe here"** at 500, **"Likely by here"** at 1,000, and **"Definitely by here"** at 2,000. That progression lines up with Isvara's 1,000-VM model, which is where the 49.4% saving appears. Neither is a SpinServers quote; both are third-party models built on their own assumptions about utilisation and staffing.

| Estate size | What the models say | What it means in practice |
| --- | --- | --- |
| Under 100 VMs | No tipping point shown at 100 | Public cloud is cheaper once 10–20 admin hours a month are counted |
| 100–500 VMs | "Maybe here" at 500 | Committed public pricing and term discounts do more for the bill than hardware does |
| Around 1,000 VMs | "Likely by here"; 23 hosts at 44 VMs each | 40–80 admin hours a month, $169,000 per host over five years including VCF |
| 2,000+ VMs | "Definitely by here" | The saving holds only while utilisation holds; empty hosts are pure loss |

The models also assume you fill the hosts. Drop from 44 VMs per host to 30 and the per-VM cost rises by roughly half, which wipes out most of the gap at the 500-VM mark.

## When private cloud is the wrong purchase

Skip it when growth is unpredictable. Capacity buffers for a demand curve nobody can forecast are the most expensive hardware you will ever buy, and the four-year refresh makes the bet worse.

Skip it when your estate needs several different stacks. Machine learning wants GPUs, SAP wants enormous memory, general IaaS wants cheap cores. No single private cloud covers all three, so you end up running and patching multiple clouds, each with its own 20-component software stack.

Skip it without an ops team. Three admins for 24/7 coverage is the floor, and 40 hours a month is not a hobby-level commitment.

Nordcloud concedes the other side of the argument in its own text: if your applications need only specific infrastructure services, their lifecycle is known and predictable, and growth is predictable too, an on-premises hyperconverged setup with private cloud features is "probably cheaper, at least for a period of time." That is a fair description of most workloads that belong on private infrastructure.

## Choosing the hardware layer

A KVM VPS is a guest on someone else's hypervisor, so it suits the workloads that made public cloud expensive: internal apps, CI runners, databases that fit in 160 GB, staging environments, backups. It does not give you a hypervisor of your own. If the plan is a real VMware or OpenStack cluster, you need bare metal, and third-party reviews of SpinServers' dedicated line describe 10 Gbps ports, IPMI remote management and ESXi auto-install, with servers in Dallas and San Jose.

As a sanity check on price, $62/month is **$744 a year for 12 vCPU and 12 GB**. That is not the same product as Isvara's $2,009 baseline VM, which carries a 0.5 TB block volume and enterprise availability, and a single VPS has no redundancy and no control plane. It is still a useful marker: 12 cores for less than half the annual price of a 4-core public cloud VM tells you how much of the hyperscaler bill is the surrounding platform rather than the silicon.

Two things to check before you buy. First, match the transfer allowance to your egress, since the plans range from 4 TB to 20 TB a month and overages are the line item that surprises people. Second, confirm the billing terms, as third-party reviews note that month-to-month dedicated orders generally carry no refund policy.

## How to decide

- **Under 100 VMs, no dedicated ops staff:** stay on public cloud and buy committed-use discounts.
- **100–500 VMs, stable application lifecycle:** get private quotes, but compare them against your committed public rates rather than list prices, and include 40 admin hours a month in the model.
- **500–1,000 VMs, predictable growth, two or three admins:** build the five-year model with the 30.7% capacity haircut, VCF licensing and a refresh line. This is the band where private usually wins.
- **GPU or large-memory workloads in the mix:** model them as a separate stack with its own hardware, not as part of the same cluster.
- **Starting from nothing:** buy one or two boxes, move the boring workloads, and measure real utilisation for a quarter before committing to 23 hosts.

Before you order, confirm three things: whether port 25 needs to be open, how much monthly transfer your workload actually pushes, and whether you need a hypervisor you control or just a VM. 👉 [Compare SpinServers' current KVM VPS plans and pricing](https://bit.ly/Spinservers)
