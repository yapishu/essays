# app school live

### lesson 0

`bowl` is a collection of env state that gets passed to a gall agent: `*bowl:gall`

minimum possible agent

```hoon
|_  bol=bowl:gall
++  on-init    `..on-init
:: on first start
++  on-save    !>(~)
:: export agent state
++  on-load    |=(vase `..on-init)
:: load agent state
++  on-poke    |=(cage !!)
:: handle poke from another agent
++  on-watch   |=(path !!)
:: handle subscriptions
++  on-leave   |=(path `..on-init)
:: handle unsubscription
++  on-peek    |=(path ~)
:: local read request
++  on-agent   |=([wire sign:agent:gall] !!)
:: communicates with other agents (subs and acks)
++  on-arvo    |=([wire sign-arvo] !!)
:: responses from vanes
++  on-fail    |=([term tang] `..on-init)
:: handles crashes
--
```

every event in arvo is atomic -- discrete, no side effects that aren't defined

```hoon
/+  default-agent, dbug
|%
+$  versioned-state
  $%  state-0
  ==
+$  state-0
  $:  [%0 values=(list @)]
  ==
+$  card  card:agent:gall
--
%-  agent:dbug
=|  state-0
=*  state  -
^-  agent:gall
|_  =bowl:gall
+*  this      .
    default   ~(. (default-agent this %|) bowl)
++  on-init
  ^-  (quip card _this)
  ~&  >  '%bravo initialized successfully'
  =.  state  [%0 *(list @)]
  `this
++  on-save   on-save:default
++  on-load   on-load:default
++  on-poke   on-poke:default
++  on-arvo   on-arvo:default
++  on-watch  on-watch:default
++  on-leave  on-leave:default
++  on-peek   on-peek:default
++  on-agent  on-agent:default
++  on-fail   on-fail:default
--
```

this agent doesn't do much more than alfa but it does introduce state

we apply (`%-`) `agent:dbug` to the stuff that follows; `state-0` defines the value, `=*` aliases `state` to `state-0` with `-`, the lark expression for the head (in this case `state-0`

we've imported `default-agent` and `dbug`; we can see `default-agent` is being used with the alias name of default; this will give us useful error messages etc without us having to implement them if we're not going to do anything with those arms

this is a slightly more complex agent; bowl is just named bowl this time

`+*` defines some aliases; we kind of set-and-forget them

`on-init` -- creates a default state with a head tag of `%0` and a bunt of a list of `@`; deferring where state comes from last night, just wrapping it and passing it

the aliasing for `card` is pretty commonly done cause we use those a lot

`dbug` is a wrapper that lets us look at the state of the agent without pokes (should always use this)

we can use dbug like `:bravo +dbug`, which will print the state

### lesson 1

a `duct` traces an event, where it's from, where it goes, and anything it needs to return (`gift`)

a `peek` (deprecated: `scry`) is a read request out of a vane; uses `.^` (dotket)

`.^(type %gx /ship/desk/case/path/mark`

- type is the return type
- next two letters are the vane and care, ie the target and what kind of request; x is peek
- ship is `(scot %p our)` unless remote scry
- desk is clay desk, care is the revision of the desk
- path is the actual scry path
- mark is only for gall peeks -- gall will attempt to cast the result to this mark

ames is the networking protocol; udp, e2e, p2p

example scry into ames: 

`.^((map ship ?(%alien %known)) %ax /=//=/peers)`

`+timers` can be used to see behn timers -- we can see the ducts that it gets returned

clay is a typed filesystem -- all files have marks, version controlled and referentially transparent, with a global namespace (ie i can address any version of any file on any ship)

marks are basically rules for converting types between each other -- a validated data type; examples in base/mar/

dill is the terminal driver -- you can `|pass [%d %text "hello mars"]` to pass a string to it; really just a keystroke handler that passes it to a userspace handler

eyre is the http server vane

hoon's subscription model is a data-flow model, meaning that when a piece of data changes, a notification is passed along a channel to subscribers who then determine what they do with it; this isn't used in most stuff in urbit but is used for frontends interacting with eyre or inter-gall comms

gall has persistent daemons in a standard format

iris is an http client but doesn't do much

jael is cryptographic secrets, mostly azimuth state, also login code (`base/gen/code.hoon`)

khan is the control plane vane, used to trigger threads from outside

you can look through `base/sys/arvo.hoon` to see what it does; it has `card`s that mean something different than in gall agents

one special thing about arvo is that every event has a history; you can always tell what triggered something (a `wire`). this isn't really the case in most OSes

### lesson 2

ipc

`[pass /some/wire %arvo %b %wait (add ~m1 now.bowl)]`

a message to behn setting a timer for a minute from now

`[%pass /some/wire %arvo %e %connect /some/url/path %some-agent]`

a message to eyre to connect an endpoint to this agent

gall agents communicate with `card`s; `note` and `gift` are the two kinds of cards; note is an outbound message being passed forward, a gift is a message being received; call/response, note/gift

pokes and peeks are one-off events (write/read to vane)

we can poke an agent like this `:alfa &noun ~` -- this addresses the agent an tells it to return a noun mark, and sending it null with sig; note that this peek doesn't do anything since alfa's on-poke arm just crashes

peeks are carried out with dotket `.^` -- example `.^(@ %gx /=alfa=/test/noun)`; alfa's on-peek does nothing so this will actually crash the dojo session :|

we're gonna have two agents that can poke each other and see the other's state, using the %charlie desk

`sur/charlie.hoon` has a structure file that gets imported by `app/charlie.hoon`:

```hoon
|%
+$  action
  $%  [%push target=@p value=@]
      [%pop target=@p]
  ==
--
```

this is a core which contains an action type, which has head tag cells(?) %push and %pop, which send a @p and an atom (target and value); %pop just puts target (@p) on its list of numbers (need to revisit whatever this means)

charlie.hoon

```hoon
/-  *charlie
:: above we import sur/charlie.hoon
/+  default-agent, dbug
|%
+$  versioned-state
  $%  state-0
  ==
+$  state-0
  $:  [%0 values=(list @)]
:: values is now a list of atoms
  ==
+$  card  card:agent:gall
:: this alias is useful because arvo has a different thing called card we want to avoid
--
%-  agent:dbug
=|  state-0
:: bunt of state-0
=*  state  -
^-  agent:gall
|_  =bowl:gall
+*  this     .
    default  ~(. (default-agent this %|) bowl)
:: defining the `this` and `default` aliases
++  on-init   on-init:default
++  on-save   !>(state)
++  on-load
  |=  old=vase
  ^-  (quip card _this)
  `this(state !<(state-0 old))
++  on-poke
:: here is where it gets interesting
  |=  [=mark =vase]
:: receives a mark and a vase (a mark and a noun)
  ^-  (quip card _this)
:: return the card and list of effects, and updated value of `this`
  ?>  ?=(%charlie-action mark)
:: assertion that checks type of incoming mark and guaranteeing it's
:: a charlie-action; distinct from structure action
  =/  act  !<(action vase)
:: extricating info from vase
  ?-    -.act
      %push
:: two actions -- %push and %pop
    ?:  =(our.bowl target.act)
:: check if we're poking ourselves
      `this(values [value.act values])
:: add this number
    ?>  =(our.bowl src.bowl)
:: if someone else is,
    :_  this
:: emit `this` as poke event
    [%pass /pokes %agent [target.act %charlie] %poke mark vase]~
  ::
      %pop
:: make sure we're targeting locally
    ?:  =(our.bowl target.act)
      `this(values ?~(values ~ t.values))
:: ?~ because if we poke an empty list, we want to emit a ~
    ?>  =(our.bowl src.bowl)
    :_  this
    [%pass /pokes %agent [target.act %charlie] %poke mark vase]~
  ==
::
++  on-peek
  |=  =path
:: takes a path
  ^-  (unit (unit cage))
:: returns a unit of a unit of a cage
:: this is so we can distinguish failure cases,
:: eg scry path doesnt exist or whatever
  ?+  path  (on-peek:default path)
:: switch on path with default
:: uses on-peek:default if no match
    [%x %values ~]  ``noun+!>(values)
:: else, return a vase of values with cen noun
:: and a unit of a unit
:: (this stuff is just boilerplate)
  ==
++  on-arvo   on-arvo:default
++  on-watch  on-watch:default
++  on-leave  on-leave:default
++  on-agent  on-agent:default
++  on-fail   on-fail:default
--
```

we can poke charlie like this :`:charlie &charlie-action [%push ~zod 100]`

to use dbug with it, we can `:charlie +dbug`

and we can look at its state with dotket: `.^((list @) %gx /=charlie=/values/noun)`

let's look at the mark file as well (`mar/charlie/action.hoon`):

```hoon
/-  *charlie
|_  act=action
:: grow/grab/grad are mostly delegated
++  grow
:: a door which takes the action as a sample
  |%
  ++  noun  act
:: just returns the instance of the action
  --
++  grab
  |%
  ++  noun  action
:: delegates the process to the action core
  --
++  grad  %noun
--
```

the basic unit of a gall subscription is the path; agent defines paths, sends facts on them; all subscribers to those paths receive those updates

an agent can't send updates to specific subscribers; if you need to differentiate subscribers you will need to give them their own paths

```hoon
|%
:: delta agents can push and pop each other
:: same as charlie
+$  action
  $%  [%push target=@p value=@]
      [%pop target=@p]
  ==
:: but there's also update
+$  update
  $%  [%init values=(list @)]
      action
:: subscribers can see action types
:: they also know about a %init type
:: because delta will report its values are
  ==
--
```

we'll also make mark files for each action in our structure files; `mar/delta/action.hoon`

```hoon
/-  *delta
|_  act=action
:: basically the same thing as above
:: except update = action
++  grow
  |%
  ++  noun  act
  --
++  grab
  |%
  ++  noun  action
  --
++  grad  %noun
--
```

and our agent, `app/agent.hoon`:

```hoon
/-  *delta
/+  default-agent, dbug
|%
+$  versioned-state
  $%  state-0
  ==
+$  state-0
  $:  [%0 values=(list @)]
  ==
+$  card  card:agent:gall
--
%-  agent:dbug
=|  state-0
=*  state  -
^-  agent:gall
|_  =bowl:gall
+*  this     .
    default  ~(. (default-agent this %|) bowl)
++  on-init   on-init:default
++  on-save   !>(state)
++  on-load
  |=  old=vase
  ^-  (quip card _this)
  `this(state !<(state-0 old))
++  on-poke
:: all this until now basically looks the same
  |=  [=mark =vase]
  ^-  (quip card _this)
  ?>  ?=(%delta-action mark)
  =/  act  !<(action vase)
  ?-    -.act
      %push
    ?:  =(our.bowl target.act)
      :_  this(values [value.act values])
      [%give %fact ~[/values] %delta-update !>(`update`act)]~
    ?>  =(our.bowl src.bowl)
    :_  this
    [%pass /pokes %agent [target.act %delta] %poke mark vase]~
  ::
      %pop
    ?:  =(our.bowl target.act)
      :_  this(values ?~(values ~ t.values))
      [%give %fact ~[/values] %delta-update !>(`update`act)]~
    ?>  =(our.bowl src.bowl)
    :_  this
    [%pass /pokes %agent [target.act %delta] %poke mark vase]~
  ==
::
++  on-peek
  |=  =path

  ^-  (unit (unit cage)

  ?+  path  (on-peek:default path)
    [%x %values ~]  ``noun+!>(values)
  ==
++  on-watch
:: this is new
  |=  =path
  ^-  (quip card _this)
  ?>  ?=([%values ~] path)
:: guarantee that the path = values
  :_  this
:: 
  [%give %fact ~ %delta-update !>(`update`[%init values])]~
:: if it does, give a fact back
:: typed delta-update, type update from structure file, type [%init values]
++  on-arvo   on-arvo:default
++  on-leave  on-leave:default
++  on-agent  on-agent:default
++  on-fail   on-fail:default
--
```

`app/delta-follower.hoon`:

```hoon
/-  *delta
/+  default-agent, dbug
|%
+$  card  card:agent:gall
--
%-  agent:dbug
^-  agent:gall
|_  =bowl:gall
+*  this     .
    default  ~(. (default-agent this %|) bowl)
++  on-poke
:: has a poke that allows it to sub/unsub from a particular agent
:: interact like: `:delta-follower [%unsub ~zod]`
  |=  [=mark =vase]
  ^-  (quip card _this)
  ?>  ?=(%noun mark)
  =/  action  !<(?([%sub =@p] [%unsub =@p]) vase)
  ?-    -.action
      %sub
    :_  this
    [%pass /values-wire %agent [p.action %delta] %watch /values]~
:: to subscribe, it `pass`es a wire named `/values-wire`, an `%agent` card,
:: which has the shape of `[action-we're-passing %agent]`, passing to %watch,
:: passing to `/values`
  ::
      %unsub
    :_  this
    [%pass /values-wire %agent [p.action %delta] %leave ~]~
:: this works basically the same as %sub
  ==
::
++  on-agent
:: 
:: this is invoked whenever we subscribe
:: that an event has happened on %delta
  |=  [=wire =sign:agent:gall]
  ^-  (quip card _this)
  ?>  ?=([%values-wire ~] wire)
  ?+    -.sign  (on-agent:default wire sign)
      %watch-ack
:: if a watch acknowledgement,
    ?~  p.sign
      ((slog '%delta-follower: subscribe succeeded!' ~) `this)
:: print this
    ((slog '%delta-follower: subscribe failed!' ~) `this)
  ::
      %kick
:: this means you lost the subscription
:: sometimes unintentional, conventional to resub at least once
    %-  (slog '%delta-follower: Got kick, resubscribing...' ~)
    :_  this
    [%pass /values-wire %agent [src.bowl %delta] %watch /values]~
  ::
    %fact
:: the thing we're giving back to subscribers
    ~&  >>  fact+p.cage.sign
:: print the fact
    ?>  ?=(%delta-update p.cage.sign)
    ~&  >>  !<(update q.cage.sign)
    `this
  ==
++  on-watch  on-watch:default
++  on-peek   on-peek:default
++  on-init   on-init:default
++  on-save   on-save:default
++  on-load   on-load:default
++  on-arvo   on-arvo:default
++  on-leave  on-leave:default
++  on-fail   on-fail:default
--
```

interact with agent like: `:delta &delta-action [%push ~zod 1.000]`

---

#### cards

`card`s are the unit of IPC -- `pass` a `card` to an agent to communicate with it

`pass` messages: `watch`,`watch-as`,`leave`,`poke`,`poke-as`

`task`s are the category that includes: 
- managing subscriptions (`watch`/`leave`)
  - `[%pass /some/wire %agent [~some-ship %some-agent] %watch /some/path]`
- `poke`s, one-off events
  - `[%pass /some/wire %agent [~some-ship %some-agent] %poke %some-mark !>(data)]`

both use a `wire` to tell the agent where to send responses (asyncronous)

pokes include a `cage` of data: a cell of a mark and a vase (`[type noun]`, produced by `!>`)

cards can be passed to arvo vanes as well:
- `[%pass /some/wire %arvo %e %connect /some/url/path %some-agent]`
- `[%pass /some/wire %arvo %b %wait (add -m1 now.bowl)]`

vanes are identified as: %a ames %b behn %c clay %d dill %e eyre %g gall %i iris %j jael %k khan

a `give` card is used to `gift`: send acks and subscription updates

`give` messages: `fact`,`kick`,`poke-ack`,`watch-ack`

acks can be for subscriptions or pokes:
```
[%give %watch-ack `~[leaf+"foo" leaf+"bar"]]
[%give %poke-ack `~[leaf+"foo" leaf+"bar"]]
```

format is `[card gift tang]`

if a poke-ack variable (tang) is null, it's positive, otherwise it's a nack -- usually the tang is an error message. gall usually handles this automatically

for subscriptions, an agent might return a `fact` or a `kick` -- an update to all subscribers for a given path, or a removal from the subscription (can be everyone on a path or specified)

#### pokes

if an agent receives a poke, it gets handled by `++  on-poke`, which accepts a cage (mark and a vase, usually individually specified) and returns a `(quip card _this)` (list of events and new state)

if a poke crashes, gall will pass the stack trace back if you use the default-agent; if you manually crash with `!!` you can specify an error message with `~|`

example for sending poke card from an agent to another agent:

```hoon
:_  this
:~  [%pass /some/wire %agent [~target-ship %target-agent] %poke %some-mark !>('some data')]
==
```

when a poke-ack gets returned, the original agent calls `++  on-agent` and tells it the wire and poke-ack (`sign:agent:gall`):

```hoon
++  on-agent
  |=  [=wire =sign:agent:gall]
  ^-  (quip card _this)
  ...
```

usually you will just pass this to default-agent but sometimes you want to look at it; test whether the tang is null to see if it's an ack or nack

example on-poke arm:

```hoon
++  on-poke
  |=  [=mark =vase]
  ^-  (quip card _this)
:: accepts a mark and vase
:: returns list of events and new state
  ?+    mark  (on-poke:def mark vase)
:: only expects %noun marks
      %noun
    =/  action  !<(?(%inc %dec) vase)
:: expects either %inc or %dec in the vase
    ?-    action
      %inc  `this(val +(val))
:: if %inc, increment value
    ::
        %dec
      ?:  =(0 val)
        ~|  "Can't decrement - already zero!"
        !!
      `this(val (dec val))
    ==
  ==
```

you could save this as `app/pokeme.hoon` and look at its state with `:pokeme +dbug [%state %val]`

to poke it, the syntax is: `:agent-name &some-mark ['some' 'noun']`

since noun is default we can omit; hence `:pokeme %inc`

you could pass cards to this agent from another agent like this:

```hoon
:_  this
:~  [%pass /inc %agent [our.bowl %pokeme] %poke %noun !>(%inc)]
    [%pass /inc %agent [our.bowl %pokeme] %poke %noun !>(%inc)]
==
```

the new agent could have an on-agent arm like this:

```hoon
++  on-agent
  |=  [=wire =sign:agent:gall]
  ^-  (quip card _this)
  ?+    wire  (on-agent:def wire sign)
:: take the wire
      [%inc ~]
:: test its value
    ?.  ?=(%poke-ack -.sign)
      (on-agent:def wire sign)
    ?~  p.sign
      %-  (slog '%pokeit: Increment poke succeeded!' ~)
      `this
    %-  (slog '%pokeit: Increment poke failed!' ~)
    `this
  ::
      [%dec ~]
    ?.  ?=(%poke-ack -.sign)
      (on-agent:def wire sign)
    ?~  p.sign
      %-  (slog '%pokeit: Decrement poke succeeded!' ~)
      `this
    %-  (slog '%pokeit: Decrement poke failed!' ~)
    `this
  ==
```

note how it tests the `wire` for the `inc` and `dec` values

##### structures and marks

we need to define a gall agent's types in the `/sur` dir

example from the previous agent:

```hoon
=/  action  !<(?(%inc %dec) vase)
```

we can see here that action is producing a vase, containing a head with a union (like an enum) of possible pokes -- we're extricating the value from the union as 'action'

example sur file for a todo app:

```hoon
|%
+$  id  @
+$  name  @t
+$  task  [=name done=?]
+$  tasks  (map id task)
+$  action
  $%  [%add =name]
      [%del =id]
      [%toggle =id]
      [%rename =id =name]
  ==
+$  update
  $%  [%add =id =name]
      [%del =id]
      [%toggle =id]
      [%rename =id =name]
      [%initial =tasks]
  ==
--
```

then we import like `/-  todo`

`mark`s are filetypes; they're specified in `/mar`; also used for for converting between marks and revision control, and for exchanging data between agents

all mark files have three arms: `grab` to convert to this mark, `grow` to convert this mark to another mark, and `grad` for revision control (don't worry about this one for now)

simple example mar file, `mar/todo/action.hoon`:

```hoon
/-  todo
:: importing the todo structure
|_  =action:todo
++  grab
  |%
  ++  noun  action:todo
:: this mark can only convert from nouns
:: passes the noun to the action struc
:: basically (action:todo [some-noun])
  --
++  grow
  |%
  ++  noun  action
:: this can only convert to nouns (already using nouns)
  --
++  grad  %noun
--
```

typically you'd want to add json arms to grow and grab for communicating with a webui

we can validate permissions of cards from foreign ships using the content of the `bowl`, which is cryptographically verified: 

- local ship only

`?>  =(src.bowl our.bowl)`

- local ship and its moons

`?>  (team:title our.bowl src.bowl)`

- particular set of ships named `allowed`

`?>  (~(has in allowed) src.bowl)`

- member of a group in group-store

`?>  .^(? %gx /(scot %p our.bowl)/group-store/(scot %da now.bowl)/groups/ship/~bitbet-bolbel/urbit-community/join/(scot %p src.bowl)/noun)`

#### subscriptions

an agent subscribes to a `path` and gets `facts` (updates) on a `wire`

incoming subs handled by `on-watch` arm; examples of path parsing in chapter 8, permissions use the `src.bowl` as above

typically updates on a path go to all subs, but the `on-watch` does allow you to send a one-time update to a new subscriber (eg initial state) by using an empty `(list path)` as below:

```hoon
:_  this
:~  [%give %fact ~ %todo-update !>(`update:todo`initial+tasks)]
==
```

(it's just `~`)

producing normal cards on a path looks like this:

```hoon
:_  this
:~  [%give %fact ~[/some/path /another/path] %some-mark !>('some data')]
    [%give %fact ~[/some/path] %some-mark !>('more data')]
    ....
==
```

any arm can send facts to subscribers, eg `on-poke`

example todo app:

- sur file defines types for agents
- mar files define filetypes (for conversion between agents)
- `app/todo.hoon` publishes agents, mostly `on-poke` actions handling stuff the action types defined in sur
    - both updates state and produces `fact`s
- `app/todo-watcher.hoon` just acts a subscriber without state, prints when it gets a card
    - uses on-poke to add or remove subs 

#### vanes

vanes take `task`s and return `gift`s

pass a task to a vane with a card: `[%pass path %arvo note-arvo]`

example behn timers:

```hoon
[%pass /some/wire %arvo %b %wait (add ~m1 now.bowl)]
[%pass /some/wire %arvo %c %warp our.bowl %base ~ %sing %u da+now.bowl /sys/kelvin]
```

`%b` is behn, `%c` is clay; the next bit is the task and parameters 

any gifts that are returned after a task go to the agent's `on-arvo` arm

#### scries

scries are read requests in the global namespace; performed with `.^`; a scry has a `care` which is the kind of request -- `%x` care is typical for gall agent scries

when a scry is delivered, all the agent receives is the care and the path -- eg `.^(update:store %gx /=graph-store=/keys/noun)` arrives as `/x/keys`

scries handled by `on-peek`, which takes a `path` and returns a `(unit (unit cage))` -- the cage contains the data, the unit has the type; response will fail without type

the easiest way to output data is:

```hoon
``noun+!>('some data')
```

examples of scry endpoints:

```hoon
++  on-peek
  |=  =path
  ^-  (unit (unit cage))
  ?+    path  (on-peek:def path)
      [%x %all ~]  ``noun+!>(data)
  ::
      [%x %has @ ~]
    =/  who=@p  (slav %p i.t.t.path)
    ``noun+!>(`?`(~(has by data) who))
  ::
      [%x %get @ ~]
    =/  who=@p  (slav %p i.t.t.path)
    =/  maybe-res  (~(get by data) who)
    ?~  maybe-res
      [~ ~]
    ``noun+!>(`@t`u.maybe-res)
  ==
```

in the full app, the `on-poke` takes `[@p @t]` and saves it in `(map @p @t)` called `data`

`/x/all`, `/x/has/<@p>`, and `/x/get/<@p>`

- /all returns `data`, the ship's state
- /has checks whether a @p is in `data` and return a bool
- /get gives the @t associated with a @p if present, or return `[~ ~]` if not

you can scry them as follows:

- `.^((map @p @t) %gx /=peeker=/all/noun)`
- `.^(? %gx /=peeker=/has/~zod/noun)`
- `.^(@t %gx /=peeker=/get/~zod/noun)`

#### failure

you can pin helper core with `+*` like so:

```hoon
=<
|_  =bowl:gall
+*  this      .
    def   ~(. (default-agent this %.n) bowl)
    hc    ~(. +> bowl)
++  on-init
```

needs to be before the rest of the agent -- note `=<`, which inversely composes the helper core and agent so that the agent isn't inside the helper's subject; from there we can define private functions for the rest of the agent (eg giving them access to the bowl)


### lesson 3

grab arm handles conversion to mark

grow handles conversions from mark

grad is for revision control (ignore for now)

ways mark files are used:
- file type handlers (`+cat /===/gen/tally/hoon`)
- for poke and peek types (`&noun` `%noun`)
- for data validators (`%charlie-action`)
- as data converters

mark files are referred to on their path as a term constant; referring to it like `%todo-action` would invoke `/mar/todo/action.hoon`

hoon represents json a little differently internally -- head-tagged cell representations:


```hoon
[ ~  
 [ %o  
     p  
   { [p='lightblue' q=[%s p='#03a9f4']]  
     [p='purple' q=[%s p='#9c27b0']]  
     [p='black' q=[%s p='#000000']]  
     [p='red' q=[%s p='#f44336']]  
     [p='indigo' q=[%s p='#3f51b5']]  
     [p='cyan' q=[%s p='#00bcd4']]  
```

this is weird but we need to decouple the meaning and representation of the data; when parsed, json turns into a unit (failure to parse will turn this will return a null, `?~` to test for this); once parsed, we can manipulate the values that are interesting to us

`%o` is the tag for objects, see it in the head of an object; the `p` means first item in a cell; `%s` is string; both sides represent strings as cords; 

if we start with json from the web, we convert it into the `$json` structure using `de:json:html`, then extract info with the `dejs:format` arms, which will convert it into specific values 

if we want to go the other way to convert something back into json, we `enjs:format` and then `en:json:html` to turn it back into a string

if we get a json string, we can `(de:json:html <var>)` to turn it into the $json struc, returning a unit; we can strip the unit stuff like `(need (de:json:html <var>))` to make the result a little simpler (strips outer cell with leading `~`)

json doesnt distinguish between integers and floats so numbers will be represented as strings; the naive way to extricate stuff out of a $json is with lark notation but that obviously sucks and it doesn't preserve type info (eg extracting a loobean will just turn into an empty string); so a reparser is necessary to convert this stuff into useful hoon representations; for reparsing you need to be cognizant of expected value types, esp ints vs floats

here is a small example of a reparser:

```hoon
=,  dejs:format
%-  ot
:~
  [%name (at ~[so so so])]
  [%member bo]
  [%dues ni]
==
```

`ot` is the parser that deals with objects; first it's going to extract the `%name` (matches head tag inside json) and assign it the type specified (`at` is array, list of 3 `so` -- @t strings); next extracts %member `bo` boolean and `ni` integer for %dues

if we save this as `reparser` and apply it `(reparser <var>)`, it will return a tuple with everything in the right place: `[['Jon' 'Johnson' 'of Wisconsin'] %.y 123]`
you could make one big reparser for everything and pull what you want out of the total result, or just pull out individual values per gate 

normally we head tag with a term in our reparser cell but we can also use a cord; necessary for mixed-case keys

our reparser's output can then be used to produce a poke to our agent, which returns a gift with a fact; the fact is converted using the mark to the %json mark, which is then turned back into a string

here we extend the `%delta` action mark's grab arm

 ```hoon
 /-  *delta
|_  act=action
++  grow
  |%
  ++  noun  act
  --
++  grab
  |%
  ++  noun  action
  ++  json
    =,  dejs:format
    |=  jon=json
    ^-  action
    %.  jon
    %-  of
    :~  [%push (ot ~[target+(se %p) value+ni])]
        [%pop (se %p)]
    ==
  --
++  grad  %noun
--
```

there's now a `++json` arm which explicitly marks this as a gate that returns an action; we use `ot`, `se`, and `ni` to read and head-tag the `target` and `value` values; or we pop the target, whichever one matches with the data (ie keep popping until you find matching)

modified update mark:

```hoon
/-  *delta
|_  upd=update
++  grow
  |%
  ++  noun  upd
  ++  json
    =,  enjs:format
    ^-  ^json
    ?-    -.upd
      %pop   (frond 'pop' s+(scot %p target.upd))
      %init  (frond 'init' a+(turn values.upd numb))
      %push  %+  frond  'push'
             %-  pairs
             :~  ['target' s+(scot %p target.upd)]
                 ['value' (numb value.upd)]
    ==       ==
  --
++  grab
  |%
  ++  noun  update
  --
++  grad  %noun
--
```

`enjs` will encode hoon values into $json marks; this is in the grow arm;

this takes in the `update`; 
- in `%pop`, `(scot %p target.upd)` turns the target into text; `pop` is put in front of it and `frond` turns the two into a single key-value pair
- in `%init` we do something similar, except `turn` with `numb` turns the whole thing into an array
- in `%push`, `pairs` does something like `frond` key-value pairs except for many instead of one

we don't have a fronted that can send anything into this yet

`;;` micmic is used to assert type validation for a value; eg if you lose type information for a noun you can reconstruct it; ex `;;(* '5')` will read the string of '5' as an atom and return 53; `;;(@ud 5)` will make it return the int