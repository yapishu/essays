## Reassembly 2023

[slide 1]

### intro

[slide 2]

hi guys, i'm ~sitful-hatred, or reid. i'm here today to represent a company
that me and three other guys run called native planet. we make hardware and
software for running urbits. we're a team of four. two of us, myself and
~dalhec-banlur, are in texas; native planet is dalhec's baby, as the original
founder who brought the rest of us on, who personally assembles your beautiful
machines in his garage like an elf. ~mopfel-winrux is in georgia, and
~nallux-dozryl is in kuala lumpur. we also have an industrial designer in
mexico who is responsible for the cool looking enclosures you may have seen.
we're an urbit-based company in the sense that we do everything on
urbit; we're basically a company run in a groups chat. the first three of us
started working on this together early last year and nallux joined us last
fall.  despite starting a hardware company in the middle of a chip shortage, we
began shipping devices this spring. today i'm going to talk about what we do,
why we do it, and what we plan on doing.

we're the urbit ecosystem's first hardware company; we sell home servers that
run like an appliance -- plug one of our handsome boxes in on a shelf in your
closet, and after some simple initial config a la chromecast, you can easily
boot and manage ships from your pc or phone with our premier software,
GroundSeg.  in technical terms, groundseg is a purpose-built container
orchestration system designed for urbit. the tag-line for groundseg is 'the
best way to run an urbit ship', and all of its features and designs are towards
that end. a few examples of those features are automated s3 hosting and
configuration, host device control functions, pier import and export, scheduled
pack and meld, and gui menus for many esoteric commands. and it's open source,
so you can install it on just about any spare pc. 

one of important features integrated into groundseg, and what i spoke about at
greater length last year, is StarTram, a purpose-built networking backend for
making home servers accessible on the public internet. GroundSeg makes running
ships easy, and StarTram lets you access them from your phone while you're out
of the house. startram is designed from the premise that setting up a
command-line app with a tls reverse proxy and port management is a nonstarter
for almost everyone who isn't an IT professional, and there needs to be an easy
way to do this. startram reduces the number of steps involved to one, which is
entering a registration password. 

we created this stack for our devices to provide a technical solution to the
problem of hosted vs self-hosted ships -- having a hosted ship is awesome
because you don't have to worry about running it. having a self-hosted ship is
awesome because your retain custody of your data. why not both?

where last year i spoke mostly about the design of startram and the value
proposition of what we were building, today i'm going to talk more about the
big picture and how we see ourselves, along with what we've been working on in
the meantime, and where we want this to go. 

[slide 3]

### philosophy

in the year or so that we've been working together, we've developed a set of
principles that act as coordination mechanisms for the design decisions we
make.  put simply, there are some ideas we think are cool, and we want the
things we make to be cool. we haven't formally explicated this anywhere so i
will try to summarize what i have internalized as the operating telos we have
arrived at:

- self-sovereignty

all of us have a gut feeling that selfhosting is the right way to do things;
what is the point of a personal server if someone else is running it? this
means custody of data, which for urbit means running your own pier. our
hardware is meant to be a home appliance that you plug in like your toaster;
our networking backend was built to allow you to run your software at home
without sacrificing ease of access or worrying about heterogeneous networking
environments. all of this is to say, your hard drive belongs at your house, and
we want to mitigate any complexity that introduces by solving the problems for
you ahead of time. ownership of your data needs to be as easy as any other
option. 

- open source

we are ideologically committed to open source software -- this is table stakes
for us. i personally would not want to run proprietary servers on my home
network, since i'm giving it a privileged position on my network, and that goes
double for anything that i use for communication. our software is permissively
licensed, i think everything on our github is MIT. 

i'll highlight that there's one exception here for our in-house managed version
of startram; but even there, we offer an open source and self-hostable version
with feature parity for individual use, called anchor. if you have a spare VPS,
or for whatever reason just really don't like us, you can easily host your own
anchor endpoint and use it just like startram. 

we're also open to feature suggestions and PRs -- i think littel-wolfur is the
only person who's taken us open on that so far, but we are always receptive to
good ideas.

- urbit maximalism

you probably won't be surprised to discover that a bunch of guys running an
urbit company together are true believers. urbit has a lot of rough edges that
need to be smoothed out, and we see our role as edge-smoothers in the service
of onboarding. 

there's still a long way to go between now and nock being the only legal form
of computation, but we want to help figure out what gets it there, whether it's
personal servers or some other value proposition we all stumble upon, though i
think it's clear we have a predisposition toward the former.

we also spend a lot of time talking about other ways to run urbit -- we've had
a lot of conversations about what it would take for a first-class mobile urbit,
other form factors, and especially iot. we have active lines of development
towards that last one, made possibe by lick, which i'll elaborate upon in a few
minutes.

- easy mode

the guiding principle of our design is that the default path should require as
little thought as possible. where complexity is unavoidable, we try to
establish sane defaults and hide stuff in the advanced settings to avoid
ho-scaring.

if you have a problem with anything, our bug report button will usually get you
the kind of customer service that will not scale in the future. under most
circumstances one or more of us will start looking into your issue as soon as
it comes in. at this point we've resolved the major classes of recurring bugs,
though, and these don't come in as often as they did at first.

there's usually a single particular thing people want out of running a ship,
which is to turn it on and access it anywhere from their phone or browser. our
'sane defaults' are meant to minimize friction for this path. as long as we're
relying on s3 for external media storage, asking people to set up and configure
it themselves can also be a tall order, since most normal humans have no idea
what s3 is or why you would need it. to that point, most people never touch a
command line in their life.  third-party hosting is the traditional answer to
this problem, but it doesn't have to be the only one.

[slide 4]

### hardware

- tellurian

our first device was the tellurian; it features a custom aluminum enclosure
with exposed board; recycled PC components, and does a fine job of running many
ships. these have dual m2 nvme drives in raid1, specifically to assure data
integrity for your pier. if you're familiar with the design firm 'teenage
engineering', you can probably tell that we are fans of them and take
inspiration from their visual style. 

my favorite part of these is the thick chassis -- photos don't do it justice,
but the have serious heft that makes it feel more like a precious object or
sculpture to me.

- aurora

premium device made in collaboration with purism, a custom hardware company
that emphasizes privacy. the aurora is basically the lambo to the tellurian's
civic.  it has lots of room for expansion, and very fast hardware. i am told
that the aurora also has an admirable heft, though i haven't held it myself
yet. if there's one thing i would like to impress upon you about our hardware,
it is that it is extraordinarily dense, and suitable for many self-defense
scenarios.

[slide 5]

- orbiter(?)

to elaborate on the car metaphor, we are also excited to release our tiny
japanese pickup, the orbiter.

if you're in our public group, you may have seen some renders of this machine.
we're planning to launch these around the end of the year. you might notice
that they're pretty small -- they're x86\_64 socs, comparable to the original
tellurians but with less memory. most importantly, we're able to get the price
point a lot lower than these -- it's too early to commit to an exact figure,
but they cost about half as much as the tellurians.

these things are also tiny! and they barely use any power. it should be
possible to run it off of a usb power bank. something that would be really cool
is a totally portable groundseg box running using a usb lte modem. this
actually should be possible with startram, so i'd really love to see someone
try it.

the neat part about shipping these is that so much of the work we do is
transferrable; there just isn't that much difference between different x64
mini-pc's to account for, especially since we build on top of docker. this
means we can focus on improving the commonly-shared software and worrying about
how charming our boxes look.

we should have some of these to show off and give away at assembly. also, we
haven't settled on the name for this thing yet, so if you have a really good
idea you should let me know, because there's a reasonable chance we'll use it.

[slide 6]

### software

groundseg is an urbit ship orchestration agent with a lot of purpose-built
bells and whistles meant to make ship management as easy as possible. groundseg
is meant to run on a dedicated device, but it doesn't have to be one of ours --
our installation script should work on most linux systems with systemd on x64
or arm64.

groundseg version 2 is a major design overhaul. we are also switching from a
rest api to websockets, which should make it feel a lot snappier. it's also
meant to decouple the backend api server from the webui to facilitate
alternative clients. our lead groundseg dev, nallux-dozryl, has been busy
hacking away at this for the last couple of months. i'd also like to take a
moment to give a shoutout to nallux, who is a very talented developer and has
delivered a lot of wins for us in between teaching us cultural lessons about
southeast asia.

probably the most exciting part of the v2 release is that we are planning to
release a new urbit app alongside it -- internally we call this gallseg, but
we'll think of something better before release. since early on, we've really
wanted to be able to control groundseg from inside a ship, but there were some
technical hurdles that kept us from doing this easily related to docker
networking. we deliberated about what it would take, and thanks to the work on
%lick by mopfel-winrux, the problems became significantly more tractable in
combination with some backend redesign. the upshot of this is that we'll be
able to control the host device from within urbit, and through your ship's
webui. 

the gallseg app will allow you to boot and control other ships on your device,
from a ship running on your device. this also opens up the possibility of
something like a self-hosted lure-equivalent. one thing worth bearing in mind
when i tell you this, is that groundseg is not just running your ship, it's
also controlling the device it's running on. for the first time, you'll be able
to, for example, reboot the host device from inside urbit, an exciting frontier
in earth-mars communication. this is also groundwork for deeper, and perhaps
device- or peripheral-specific integrations in the future.

startram, like i described earlier, is the networking backend we built for
making ships publicly accessible. most obviously this means the web interface
gets a public URL, but we also automatically forward ames packets. this backend
is generalizable to other kinds of traffic -- we like to think of our boxes as
home servers in the general sense, though our plans remain oriented around
urbit functionality. in particular, we intend to make home-hosted webrtc as
easy as we make s3 buckets, at least on the smaller end of the scale.

we've also started some tentative work on moving some of our cicd
infrastructure into urbit -- nothing crazy to start with, basically just
replacing what are presently get requests for json over http. the guiding
principle of these exploratory projects is to minimize the surface area between
earth and mars and find out what can be practically brought over.

aesthetically, i think there's something to be said for trying to swallow what
we can from earth. we use a lot of conventional software for our
infrastructure, but we're ideologues, and we have a deep appreciation for
pushing urbit where we can. 


[slide 7]

### ecosystem

our cto, mopfel-winrux, has been busy contributing to urbit core development.
this is especially useful for us since he has a close eye on new developments
which gives us time to prepare for changes or adapt to new features, but he's
also a skilled engineer who has a lot of feathers in his cap. in particular
i'll highlight a couple of projects he's worked on:

%lick is a vane for hardware control that allows a ship to communicate through
unix IPC and lets you poke stuff on earth. this is, i believe, the first vane
written by a non-core dev, and will ship with the next binary release, i think
next week.  the lick project was undertaken for a UF grant, and the final demo
was a thermal printer that was controlled from mopfel's ship; he followed this
up with a python gui snake game called slick, built on top of a hoon snake game
originally made by palfun.  you can see some paths forward from here that we're
interested in pursuing, like driving displays or peripherals.

like i mentioned earlier, lick has made it possible for us to write urbit
software that crosses the hermetic seal and manage conventional computers. we
see this as a critical foundation for the kinds of things we want to do in the
future -- we want as many devices as possible to run urbit, or at least be
managed by devices that do.

mopfel's also been contributing to lagoon and saloon with neal, which are
libraries for linear algebra and matrix multiplication. this is groundwork for
whatever future for whatever the future holds for urbit and machine learning,
which is something we're also interested in accommodating, though it is still
early days.

[slide 8]

### nockpu 

here's an exciting reassembly exclusive announcement. those of you who know
mopfel-winrux probably know him as the guy behind the nockpu project. if you're
not familiar, nockpu is the first serious attempt to design a computer meant to
execute the nock ISA directly rather than virtualizing it, like we do when we
run a ship with vere. this is how i first came to know of him, anyway, and was
the biggest reason i wanted him to join us for the native planet project.  this
is implementing nock at the transistor level, something that's been a bit of a
pipe dream for as long as i've been around this scene. the ultimate goal here
would be something like a nock coprocessor, where you can shove a subject at it
to evaluate and hopefully it can do it faster than your CPU. 

this kind of work is not easy, though, and this is something that he's been
working on in his spare time in between native planet work, and general urbit
work, and doing his day job. as a result, progress has been slow. but, thanks
to a research grant by our very own generous host jae, mopfel will be able to
focus his full efforts on this project for long enough to see it to completion,
and deliver the first physical nockpu. this is meant to go all the way from
verilog schematics to a burned fpga.

mopfel wanted me to impress upon you that the end of this project is not a
consumer product; this is a research project to figure out how this can be
done. this project is meant just as much to figure out what challenges nock has
in hardware as it is to program an FPGA. it might turn out that a conventional
embedded interpreter is inherently more efficient. but, like some of the other
things i've mentioned today, this is foundational work for determining what it
is possible to develop in the future. and if anyone on the network can do this,
mopfel is the guy. 

[slide 9]

### end

there's more ideas and projects that we're thinking about or exploring that i
haven't talked about today, but we don't want to overpromise things we're not
already pretty certain about -- with that in mind, though, i hope that gives
you an idea of what we're thinking about and where we're headed. i personally
am very proud of what we've done at native planet and i'm very excited about
where this is headed.
