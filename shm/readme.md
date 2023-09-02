# Self-hosting manifesto

## theory: why we host

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

Digital technology seems mundane to us today -- most of us were born into a
time when it was already mature and pervasive. Do not let this feeling lead you
to complacency; we live in an era of magic. The powers available to Joe Nobody
include zero marginal cost replication of information, effortless global
communication, lossless archival and transmission of speech and images, and
other wonders unimaginable to our ancestors. Encryption, the ability to keep a
secret from observers, similarly has no prior parallel. This is effectively the
power to conspire, enshrined in code as a natural right, and available to any
who are willing to take it up. 'Conspire' might sound devious, but it is a
fundamental freedom -- private coordination.

'Security through obscurity' gets trotted out as a design fallacy, a cargo cult
performance of actual security, but it remains an important part of a
defense-in-depth. As a general principle, information asymmetry advantages one
party over another. Observation allows for prediction. Being hidden, being hard
to understand, can be a powerful shield; "being described is the prelude to
being attacked." 

The US Federal government is estimated to classify 50 million documents a year;
you are already in an arms race, even if you don't know it. This analogy runs
both ways -- in an attempt to roll back public advances, strong cryptography
was classified as a munition by the US State Department and remains subject to
export restrictions. They don't want you to know this, but your ALU is a
weapon.

Trusting your tools enough to defend yourself with them means being able to
understand them well enough to model their behavior -- allowing someone else to
run your tools behind some proprietary commercial service is effectively
opting-out of the possibility. 

The best way to understand a tool is to use it. Our goal is to put the tools in
your hands.

Our moment is remarkable as a threshold between ages -- the last historical
period with no distinction between humans and agency; après moi, le réseau.
Going forward, what was once an ambiguously hostile but fundamentally
comprehensible social environment will become increasingly dominated by a
kaleidoscope of adversarial nonhuman agents. These agents are composed of
probabilistic associations extracted from aggregated data. The best time to
take up these tools and withdraw your data from of the corpus of training
material was yesterday; the second-best is now.

Native Planet is an ideologically-motivated project. Our social coordination
nexus -- our cooperative Schelling point -- is the desire to make self-hosting
easier and better than letting someone else run software for you. The price of
the latter has become clear -- be observed, monitored, bucketed, modeled. The
advantage of the former will become increasingly obvious as time goes on.

> i have seen many people spill their guts on-line, and i did so myself until,
> at last, i began to see that i had commodified myself. commodification means
> that you turn something into a product which has a money-value. in the
> nineteenth century, commodities were made in factories, which karl marx
> called “the means of production.” capitalists were people who owned the means
> of production, and the commodities were made by workers who were mostly
> exploited. i created my interior thoughts as a means of production for the
> corporation that owned the board i was posting to, and that commodity was
> being sold to other commodity/consumer entities as entertainment. that means
> that i sold my soul like a tennis shoe and i derived no profit from the sale
> of my soul. people who post frequently on boards appear to know that they are
> factory equipment and tennis shoes, and sometimes trade sends and email about
> how their contributions are not appreciated by management.

> as if this were not enough, all of my words were made immortal by means of
> tape backups. furthermore, i was paying two bucks an hour for the privilege
> of commodifying and exposing myself. worse still, i was subjecting myself to
> the possibility of scrutiny by such friendly folks as the FBI: they can, and
> have, downloaded pretty much whatever they damn well please. the rhetoric in
> cyberspace is liberation-speak. the reality is that cyberspace is an
> increasingly efficient tool of surveillance with which people have a
> voluntary relationship.

-- *humdog*, 1994

---

## praxis: they don't want you to know this, but every computer is a server

### how to run a node in the den of your abode

Most PCs and single board computers manufactured within the last ten years are
well-suited to be converted into a sovereign personal server. Understanding the
minimum technical requirements, choosing an operating system, and understanding
remote access and controlare the keys to getting a personal server up and
running. You an build a server from new components from scratch, or act
resourcefully and build a perfectly capable server from parts and old devices
you have laying around. 

### Selecting hardware

Don't overlook the potential lying dormant in old laptops, retired desktops, or
unused motherboards. Reclaiming junked or unused hardware not only saves
resources, but in some cases may be your only viable option. Most computers are
capable of most things that you want to use them for.

#### Laptops

Laptops may not be the first thing that 'personal server' conjures to mind, but
they carry some advantages.

**All-in-One** -- laptops are self-contained units with built-in display
screens, keyboards, and trackpads. This can be useful in circumstances where
you need direct access, like during initial setup or network outages.

**Power failover** -- Laptops are equipped with batteries, which let it keep
running during power interruptions. In case of outages or fluctuations, your
server can continue running smoothly, ensuring uninterrupted access.

**Resource Efficiency** -- While old laptops might not match the processing
power of modern systems, they are more than capable of handling basic server
tasks such as file sharing, long-running background tasks, and P2P
communications.

#### Desktop PCs

Repurposing an old PC as a personal server offers some distinct advantages over
using a laptop or a single-board computer. They can often be obtained cheaply
-- look on ebay for used micro PCs.

**Power** -- PCs typically have the most powerful CPUs of any of the categories
of devices used for personal servers (when compared to devices manufactured in
the same period). This can be useful if your desired use-case involves heavy
processing loads or many concurrent services, but comes with the tradeoff of
increase power consumption.

**Expandability** -- PC boards often have greater support for modular component
replacement and upgrades. That middling microtower might be more than enough to
act as a media server if you double the memory, and multiple disk drives can be
useful in many contexts.

**Compatibility** -- Commodity X64 PCs are typically well-supported by whatever
OS you put on it, since Linux has robust support for most PC hardware.

#### Single Board Computer (SBC)

Opting for a Single Board Computer (SBC) such as a Raspberry Pi or a Latte
Panda x86 as your personal server introduces its own advantages that set it
apart from using a laptop or an old PC. 

**Compact** -- you can hide these in your building's ventilation.

**Efficient** -- SBCs use very little power, and are typically designed to run
on power carried over USB -- a few watts at ~5A is common, on par with a cell
phone charger. This is low enough to run on auxiliary power sources for
prolonged periods.

**Cheap** -- You can usually get one of these new for less than half the cost
of even a cheap desktop PC. Personal servers don't need to be able to play CoD.

**Purposeful** -- SBCs often have hardware accomodations for particular
use-cases, like network-attached storage, serving media, or IoT. This can
reduce complexity and cost if it aligns with your desired use-case.

## Minimum specs

Depending on the type of device you decide to use and how you plan to use your
server, your minimum required specifications may vary. These are suggestions
for general minimum specifications to ensure a lightweight and efficient
personal server that fulfills the basic needs of sovereign peer-to-peer
communications.

#### Memory (RAM)

Memory is a critical pillar of your server's level of capability, which defines
a soft cap on the volume and complexity of tasks it's capable of. Remember the
simple mantra: "more memory is good".

**Minimum Requirement**: 2GB

**Recommended**: ≥8gb

#### CPU

CPUs are the main bottleneck on operational speed, but are notoriously
difficult to directly compare to one another, since many variables must be
accounted for -- power draw, clock speed, core count, etc. It's best to bucket
them by manufacturing date and general categories, like "2020 mobile processor"
or "2017 desktop CPU", before making comparisons. 

SBCs and Laptop boards will always have an integrated processor, which should
be known and considered when you make the selection. OEM desktops are usually
built around a particular CPU class or model but can be swapped out for another
unit with the same socket specification.

Generally speaking: newer is better. Moore's law holds, and a CPU released in
2022 is almost always going to be better than one from 2015. 

### Disk

Disk is long-term storage for your device -- all content that persists between
reboots. Some services require significant disk space, but many uses require
little or no space to operate continuously for years. However, running services
like Urbit or media servers require ample room for growth.

We strongly recommend solid state drives over SD cards or mechanical hard
drives as they are more durable and less susceptible to failure. You can
connect your SSD to your motherboard via USB, PCIE, or SATA connection -- while
all of these will work, a PCIE or SATA connection is preferred as these methods
will ensure you take full advantage of the speed of your SSD. 

**Minimum Requirements**: 128GB SSD

**Recommend Requirements**: ≥500GB SSD

### Open Source Operating Systems & Software

- **Linux** -- almost any personal server software you run is going to be built
  on top of Linux, whether you're aware of it or not. Gaining basic familiarity
  with operating a Linux device, especially from the command line, is a major
  capability boost and powerful tool that makes many novel tasks tractable.
  Everything else on this list is built on Linux, as it is the de facto
  standard for free computing.

- **ColonyOS** -- Native Planet's purpose-built Linux distribution for running
  Groundseg (see below). Includes optional desktop environment, but designed
  for 'headless' (remote, monitorless) usage.

- **Urbit** -- A new internet from first principles. Urbit is an operating
  system that runs on top of conventional server OSes and allows you to
  maintain total ownership of your data and communications. Urbits have a
  permanent identity and properties that allow it to belong to you in ways that
  conventional software and computers cannot.

- **GroundSeg & StarTram** -- Your server's Urbit orchestration layer.
  GroundSeg is software designed to run Urbit on home PCs and abstract away the
  complexities of Linux system administration. StarTram is the networking
  backend that makes the web interface and hosted services accessible from
  anywhere, available as a managed subscription service or a self-hosted
  minimal-config deployment ("Anchor").

- **Umbrel** -- Originally a personal server software suite designed around
  Bitcoin, Umbrel now aims to be a general-purpose Linux server administration
  framework and control center. Supports many popular self-hosted software
  packages like cryptocurrency tools, media centers, and social software.

- **Casa OS** -- Similar to Umbrel, another general-purpose personal server
  suite meant for running conventional Linux softwar easily, aimed primarily at
  ZimaBoard SBC personal servers.

### NAT, Relays and Firewalls

Hosting a personal server at home requires more than just installing software
on a computer on your local network. Due to the finite nature of IPv4 address
space, the devices running on your home network have local, or private IP
addresses that by default aren't reachable by devices outside of that network.
When it comes to making your software reachable from the broader internet, you
have several options:

- Configuring networking rules
- Cloud deployment
- Private VPNs
- Relaying your connection

Configuring your own network is the most resourceful option -- if you
understand the problem you're solving, you can configure static IP addresses
and port forwarding rules to allow predetermined traffic patterns to flow
across your network boundary in a defined way to make specific services
publicly reachable.

Running your software in the cloud sidesteps some of this complexity -- this
tends to mean running your software in the cloud on a VPS (virtual private
server). This option reduces the complexity of software deployment in some
ways, but also means absolving yourself of sovereignty over your software.
Instead of running your own services, you're renting CPU time on a mainframe.

Private VPNs, like Tailscale, allow you to address the problem of public access
without inheriting the security or privacy considerations that may entail.
Tailscale is a networking framework built on top of the Wireguard VPN protocol,
that allows you to create private networks that are reachable from the public
internet. Instead of publicly exposing your service, you can connect to a
private network of your devices running the Tailscale client software (or an
equivalent) and then connect to your devices via their private addresses on a
virtual LAN. This has advantages, but maybe a non-starter for some purposes
that require public connectability.

Relaying your connection is a final option -- GroundSeg's StarTram service,
also built on top of Wireguard, forwards defined services to a
publicly-connectable endpoint at will, allowing you to control which services
public and which are not. In GroundSeg, this means giving Urbit ships a public
subdomain with encryption, tunneled over another connection that is itself
encrypted. Additionally, sidecar services like self-hosted S3 buckets for media
hosting are included out of the box, allowing you to host media for your
communities without relying on a third party. This option requires the least
intervention on your part.

# Miscellaneous tips

- **Smart plugs** are useful for controlling power to devices and peripherals.
  These alow you to turn devices on and off remotely.

- **Bypass power buttons** in your system BIOS. Most commodity PC hardware
  allows you to set your device to turn on immediately without intervention,
  which you can make use of in conjunction with remotely controlled smart
  plugs.

- **Hardwire your network connection rather than relying on WiFi**: WiFi is
  useful for client devices that don't need consistent uptime or are only in
  use some of the time. Servers use uptime as a performance indicator -- they
  need to be reachable and responsive as close to "always" as possible. WiFi
  introduces latency and layers of complexity that act as points of failure,
  which can impact the reliability of your servers. Tend toward wired
  connections whenever possible.

- **USB Cellular Modems** -- Sometimes a wired connection isn't possible, and
  even a WiFi connection is difficult to guarantee. In these situations, USB
  LTE modems can provide connectivity for your devices, though this can become
  expensive for high-throughput tasks. 

- **Ignore all of our advice** -- there are no rules. Any computer is a server.
