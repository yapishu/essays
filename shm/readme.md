# Self-hosting manifesto

## part 1: why we host

Feedback loops between technology and social organization are a throughline in
history. Technology might be purely social -- a set of written or informal
social contracts and axioms, a tradition, a metaphysics, acting as a Schelling
point for coordination. Coordination is built on trust; trust is built on
predictive modeling, and shared assumptions lend greater confidence to modeled
probabilities.

At an increasing tempo, the development and adoption of the social technologies
that shape our modes of organization reflect material technologies -- the
printing press breaks the clerical monopoly on meaning and narrative, kicking
off a century of realignment and bloodshed. Industrial output saturates global
commercial flows and depopulates the countryside. Electrical communication
collapses distance and time, pulling the periphery into the core and
homogenizing language and discourse.  Computer networks act as exponential
amplification circuits on prior cycles as they expose and reorient information
flows across social graphs. Cognition has decoupled from sentience and become a
metered utility, presaging even deeper centripetal cycles of intensification. 

The impact of material technologies might be measured by a polarity between
entrenching or disrupting a social order. A dispruptive technology par
excellence is the rifle -- a millenium of hereditary martial aristocrats
outmoded as a class by whoever can field the most conscripted laborers,
traditional societies across the globe overthrown and brought under the
gunpowder empires, and centuries of advanced states pushed out of territories
by motivated and coordinated riflemen. History only presents a few moments like
this, where the the leverage offered by a new technology is broadly available
to individuals, exerts centrifugal force on existing power structures, and
resists being put back into the box. 

The position being put forth here is that right now, you are living in a period
of such technology, with access to tools that represent an equivalent rupture.
You have access to powers that could only be described as magic a few decades
ago. Observing the currents in the moment you exist in, and the methods of
maximum leverage available to you, are necessary to preserve and compound your
agency across time. Most of these tools are symmetrical at best vis-a-vis your
relationship with other actors, or are disfavorable to you -- think of data
aggregators and markets that serve only states and corporate analytics
engineers -- but the field of privacy-oriented tech, built on the foundation of
modern cryptography, is the contemporary rifle, or the equivalent of the
printing press.

'Security through obscurity' gets trotted out as a design fallacy, but it
remains an important part of a defense-in-depth. As a general principle,
information asymmetry advantages one agent over another. Observation allows for
prediction. Being hidden, being hard to understand, can be a powerful shield;
"being described is the prelude to being attacked." The US Federal government
is estimated to classify 50 million documents a year; you are already in an
arms race, even if you don't know it. This analogy runs both ways -- in an
attempt to roll back public advances, strong cryptography was classified as a
munition by the US State Department and remains subject to export restrictions.
They don't want you to know this, but your ALU is a weapon.

Trusting your tools enough to defend yourself with them means being able to
understand them well enough to model their behavior -- allowing someone else to
run your tools behind some proprietary commercial service is effectively
opting-out of the possibility. 

The best way to understand a tool is to use it. Our goal is to put the tools in
your hands.

Native Planet is an ideologically-motivated project. Our social coordination
nexus -- our cooperative Schelling point -- is the desire to make self-hosting
easier and better than letting someone else run software for you. The price of
the latter has become clear -- be observed, monitored, bucketed, modeled. The
advantage of the former will become increasingly obvious as time goes on.

Our moment is remarkable as a threshold between ages -- the last historical
period with no distinction between humans and agency; après moi, le réseau.


> i have seen many people spill their guts on-line, and i did so myself until,
> at last, i began to see that i had commodified myself. commodification means
> that you turn something into a product which has a money-value. in the
> nineteenth century, commodities were made in factories, which karl marx
> called “the means of production.” capitalists were people who owned the
> means of production, and the commodities were made by workers who were
> mostly exploited. i created my interior thoughts as a means of production
> for the corporation that owned the board i was posting to, and that
> commodity was being sold to other commodity/consumer entities as
> entertainment. that means that i sold my soul like a tennis shoe and i
> derived no profit from the sale of my soul. people who post frequently on
> boards appear to know that they are factory equipment and tennis shoes, and
> sometimes trade sends and email about how their contributions are not
> appreciated by management.

> as if this were not enough, all of my words were made immortal by means of
> tape backups. furthermore, i was paying two bucks an hour for the privilege
> of commodifying and exposing myself. worse still, i was subjecting myself to
> the possibility of scrutiny by such friendly folks as the FBI: they can, and
> have, downloaded pretty much whatever they damn well please. the rhetoric in
> cyberspace is liberation-speak. the reality is that cyberspace is an
> increasingly efficient tool of surveillance with which people have a
> voluntary relationship.

---

## part 2: they don't want you to know this, but every computer is a server

### how to run a node in the den of your abode

Most PCs and single board computers manufactured within the last ten years
are well-suited to be converted into a sovereign personal server. Understanding
the minimum technical requirements, choosing an operating system, and
understanding remote access and controlare the keys to getting a personal
server up and running. You an build a server from new components from scratch,
or act resourcefully and build a perfectly capable server from parts and old
devices you have laying around. 

### Selecting hardware

Don't overlook the potential lying dormant in old laptops, retired desktops, or
unused motherboards. Reclaiming junked or unused hardware not only saves
resources, but in some cases may be your only viable option. Most computers
are capable of most things that you want to use them for.

#### Laptops

Laptops may not be the first thing that 'personal server' conjures to mind,
but they carry some advantages.

**All-in-One** -- laptops are self-contained units with built-in display 
screens, keyboards, and trackpads. This can be useful in circumstances where
you need direct access, like during initial setup or network outages.

**Power failover** -- Laptops are equipped with batteries, which let it keep 
running during power interruptions. In case of outages or fluctuations, 
your server can continue running smoothly, ensuring uninterrupted access.

**Resource Efficiency** -- While old laptops might not match the processing power
of modern systems, they are more than capable of handling basic server tasks
such as file sharing, long-running background tasks, and P2P communications.

#### Desktop PCs

Repurposing an old PC as a personal server offers some distinct advantages over 
using a laptop or a single-board computer. They can often be obtained cheaply -- 
look on ebay for used micro PCs.

**Power** -- PCs typically have the most powerful CPUs of any of the categories
of devices used for personal servers (when compared to devices manufactured in
the same period). This can be useful if your desired use-case involves heavy
processing loads or many concurrent services, but comes with the tradeoff of
increase power consumption.

**Expandability** -- PC boards often have greater support for modular component
replacement and upgrades. That middling microtower might be more than enough
to act as a media server if you double the memory, and multiple disk drives
can be useful in many contexts.

**Compatibility** -- Commodity X64 PCs are typically well-supported by whatever
OS you put on it, since Linux has robust support for most PC hardware.

#### Single Board Computer (SBC)

Opting for a Single Board Computer (SBC) such as a Raspberry Pi or a Latte
Panda x86 as your personal server introduces its own advantages
that set it apart from using a laptop or an old PC. 

- **Compact** -- you can hide these in your building's ventilation.

- **Efficient** -- SBCs use very little power, and are typically designed to
run on power carried over USB -- a few watts at ~5A is common, on par with a
cell phone charger. This is low enough to run on auxiliary power sources for
prolonged periods.

- **Cheap** -- You can usually get one of these new for less than half the cost of
even a cheap desktop PC. Personal servers don't need to be able to play CoD.

- **Purposeful** -- SBCs often have hardware accomodations for particular use-cases,
like network-attached storage, serving media, or IoT. This can reduce complexity and
cost if it aligns with your desired use-case.

## Minimum specs

Depending on the type of device you decide to use and how you plan to use your
server, your minimum required specifications may vary. These are suggestions for
general minimum specifications to ensure a lightweight and efficient personal
server that fulfills the basic needs of sovereign peer-to-peer communications.

#### Memory (RAM)

Memory is a critical pillar of your server's level of capability, which defines
a soft cap on the volume and complexity of tasks it's capable of. Remember the
simple mantra: "more memory is good".

**Minimum Requirement**: 2GB

**Recommended**: ≥8gb

#### CPU

CPUs are the main bottleneck on operational speed, but are notoriously difficult
to directly compare to one another, since many variables must be accounted for --
power draw, clock speed, core count, etc. It's best to bucket them by manufacturing
date and general categories, like "2020 mobile processor" or "2017 desktop CPU",
before making comparisons. 

SBCs and Laptop boards will always have an integrated processor, which should be
known and considered when you make the selection. OEM desktops are usually built
around a particular CPU class or model but can be swapped out for another unit with
the same socket specification.

Generally speaking: newer is better. Moore's law holds, and a CPU released in 2022 
is almost always going to be better than one from 2015. 

### Disk

Disk is long-term storage for your device -- all content that persists between
reboots. Some services require significant disk space, but many uses require little
or no space to operate continuously for years. However, running services like
Urbit or media servers require ample room for growth.

We strongly recommend solid state drives over SD cards or mechanical hard drives 
as they are more durable and less susceptible to failure. You can connect your 
SSD to your motherboard via USB, PCIE, or SATA connection -- while all of these 
will work, a PCIE or SATA connection is preferred as these methods will ensure 
you take full advantage of the speed of your SSD. 

**Minimum Requirements**: 128GB SSD

**Recommend Requirements**: ≥500GB SSD

### Open Source Operating Systems + Software

- **Linux** -- almost any personal server software you run is going to be built
on top of Linux, whether you're aware of it or not. Gaining basic familiarity with
operating a Linux device, especially from the command line, is a major capability
boost and powerful s

- Colony OS

- GroundSeg & StarTram

- Casa OS

- Umbrel

- ClearOS

#### VPNs, NAT Traversal, and Third Party Relays

In today's digital landscape, the importance of VPNs (Virtual Private Networks)
cannot be overstated when it comes to hosting your own personal server. A VPN
establishes a secure and encrypted connection between your server and the
outside world, shielding your data from prying eyes and potential cyber
threats. By masking your server's true IP address, a VPN ensures your online
activities remain confidential and your server's vulnerabilities are
significantly reduced. This layer of protection not only safeguards your
personal information but also bolsters the security of the data stored on your
server, making VPNs an indispensable tool for responsible and secure
self-hosting.

Router punching, another critical aspect of self-hosting, plays a pivotal role
in overcoming the challenges of network configurations and firewall
restrictions. It enables your server to establish direct connections with
external devices even if they are situated behind routers or firewalls. By
skillfully navigating these barriers, router punching ensures seamless and
efficient communication between your personal server and its users,
irrespective of their geographic locations. This capability not only enhances
user experience but also streamlines the accessibility of your hosted services,
making them readily available to a wider audience.


The integration of third-party relays, such as Native Planet's innovative
StarTram service, amplifies the reliability and reach of your self-hosted
server. These relays act as intermediaries, facilitating communication between
your server and users by efficiently relaying data across networks. StarTram,
for instance, boasts a global network of high-speed servers strategically
positioned to optimize data transmission. Leveraging such services optimizes
server performance, minimizes latency, and ensures consistent access even in
regions with varying internet speeds. By incorporating third-party relays,
self-hosted servers can achieve unprecedented levels of accessibility,
resilience, and user satisfaction, underscoring their crucial role in the
modern era of personal server hosting.

# Helpful advice

SmartPlugs

Bypass power buttons in system BIOS

Hardwire to the Internet rather than rely on WiFi

USB Cellular Modems
