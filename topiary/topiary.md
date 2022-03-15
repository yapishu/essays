# A Topiary: Hypertext and Urbit


> Under the trees of England I meditated on this lost and perhaps mythical labyrinth. I imagined it untouched and perfect on the secret summit of some mountain; I imagined it drowned under rice paddies or beneath the sea; I imagined it infinite, made not only of eight-sided pavilions and of twisting paths but also of rivers, provinces and kingdoms... I thought of a maze of mazes, of a sinuous, ever growing maze which would take in both past and future and would somehow involve the stars.


-- *The Garden of Forking Paths*, Jorge Borges




	 	 	 	
The pre-technological world was already [networked](https://en.wikipedia.org/wiki/Indra%27s_net). No material object and no idea has ever stood alone—"no man is an island". Language itself is a network of associations shared between a group of people. The question of technology is: how do we bring our web of associations into the realm of computers? The legacy internet has focused on social networks almost exclusively—our mammalian brains readily took to this, we all love to [signal](https://conversationswithtyler.com/episodes/robin-hanson/). But before “social media” there was something called hypertext: a seemingly modest way to connect documents shared between computers.

A footnote in a book used to require a library, locating the referenced book, and finding the cited page (which might have footnotes itself)! Hypertext allows one to traverse a network quickly and easily with just mouse clicks, making information accessible to those without the time and resources to hunt down physical artifacts. Urbit’s “%graph-store” is not the same as hypertext but it does bear some familial resemblances. Fundamentally, %graph-store is a data structure that can accommodate disparate data types within a single edifice. It is based on [graph databases](https://en.wikipedia.org/wiki/Graph_database) and recent implementations such as [GraphQL](https://graphql.org/). To understand this data structure it is worth diving into the origins of hypertext—the original linked network.

The story of hypertext begins at the end of WWII with an article in *The Atlantic* that proposed to extend the human mind via machine. Vannevar Bush had seen first-hand the explosion of electronics manufacturing and scientific knowledge, recognizing the great peril and opportunity of such a revolution. His [description](https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/) of the Memex presaged the technological world of today, envisioning a personal device that would allow the user to draw upon a vast library of information, and link documents together arbitrarily—what he called "associative indexing"—a deliberate recreation of how the mind connects thoughts.


The 1960s saw the first implementations of these ideas. Ted Nelson's [writing](https://archive.org/details/SelectedPapers1977/page/n15/mode/2up) around *Project Xanadu* elaborated this linking between documents into what Nelson called *hypertext*. Nelson imagined "a text arranged in a graph structure", centered around a corpus arranged in nonlinear sequence, manipulable by author or audience. The concept was first demonstrated in 1968, at Douglas Engelbart's 'Mother of all Demos', where a vision of computers as a collaborative social environment was revealed to the world. "[The Journal](https://www.dougengelbart.org/content/view/137/#7)", presented alongside other revolutionary demos, exhibited collaboratively edited documents with hyperlinked connections. Any time-travellers in the audience would have immediately recognized the first webpage. From here, the idea of a linked network of documents would be developed through successive phases of computing, from the mainframe to the PC.
 

Experimentation with these ideas continued through the 1970s and 80s. A team directed by Andy Lippman at MIT extended the concept to threaded visual hypermedia with the *Aspen Movie Map*, a user-directed virtual tour of Aspen, Colorado. Hypertext and hypermedia gained a mass audience for the first time with HyperCard, a program that shipped with Mac computers. Users were able to collect or create '[stacks](https://archive.org/details/hypercardstacks
)' of interlinked cards; a personal body of text, art, and knowledge could be accrued and modified, or shared with others—though not yet with native networking capabilities. 


With the World Wide Web, hypertext achieved escape velocity. Initially a one-man [project](http://info.cern.ch/hypertext/WWW/TheProject.html), the world wide web was similar to Bush’s vision of the Memex—a way to share, index, and connect information between individuals and groups. The web succeeded by utilizing the physical infrastructure already available: PCs with graphical interfaces, DNS, and the internet. Critically, its method of transmitting and traversing texts was a standardized protocol, allowing a free body of information to assemble atop the new infrastructure by anyone willing to learn the tools. A new frontier was cleared and cultivated with open standards, but it would gradually face enclosure.


In the following decade, after the first wave of social networking sites had crested but before phones became the primary web clients, Twitter was launched. Originally an awkward bridge between SMS and the web, its design eventually coalesced around static identity, posts with chronological sorting, and interlinked post threads. These simple primitives spawned a vast corpus of information, a graph-of-graphs intertwined and overlapped, mapping an entire platonic world. Twitter is fundamentally constrained by its very nature: it is the walled garden of an advertising company. The financial incentives of the server operators demanded seizure of the commons and the replacement of standardized protocols with proprietary databases.


As the first networked services withered, a new critique was implicitly articulated in the construction of a [new kind of abstract machine](https://moronlab.blogspot.com/2010/01/urbit-functional-programming-from.html). Urbit learned from the past: tightly defined protocols, baked-in cryptography, and decentralization were key. If power over the network was determined by those who ran the servers, then we would run the servers ourselves. 


Urbit is not fundamentally a hypertext system; it is a new kind of computer, designed to participate in a peer-to-peer network that rejects the client-server model. All Urbit software takes its networking model for granted, it is not an ad-hoc layering of mismatched abstractions. Fundamental design decisions determine the trajectory of the future.


Early [implementations](https://en.wikipedia.org/wiki/Neo4j) of graph databases and projects like [RDF](https://en.wikipedia.org/wiki/Resource_Description_Framework) hit on the same core idea: linked data needs its own data structure. In 2020 Urbit OS adopted an application called %graph-store that made use of a new, native [data structure](https://docs.google.com/document/d/1-Gwfg442kV3cdfG7NnWPEf2TMa3uLUTAKkZD70ALZkE/edit) which would act as a database service for social applications. Chats, long-form text, and other types of data produced in the course of social computing now share the same tent, a single structure extensible to new types of content. Each item added to it is a node in a personal graph—a tree that grows branches as your computer communicates. These nodes can be linked arbitrarily to other graphs—Urbit's global immutable namespace grants both permanence and ownership and allows the graph’s branches to intertwine with others. Graphs can accommodate a variety of content, from simple text, to code and media yet to be conceived. You own your graph, and it is a record of your digital journey with others.


Personal hypertext graphs are leaving behind proprietary platforms and gaining independence. Graphs until now have been used as tools of surveillance and marketing, but they are being reclaimed and sewn back into the network, each bound to the cryptographic identity of an individual, permanent computer. Where the map of your life was once splintered across a thousand systems and held in the custody of strangers, it is now possessed only by you and shared only as you see fit. On Urbit, an infinite forest of digital lives can take root, nesting their boughs and stalks.



