

okay so I want to set a few
expectations.

I wasn't I wasn't sort of expecting to

stand at the front and give any sort of

talks so not very much is is is ready in

that regard it's really meant to be a

workshop for kind of rough coding I

wanted to get some people whoever

brought their laptop sort of up to
speed

maybe get substrate downloaded and
built

and sort of take take people through

maybe deploying deploying a new chain

maybe writing some modules Sergei here

is going to sort of give some give a

demonstration of how to deploy a smart

contract on our smart contract based

substrate base smart contract chain so

yeah don't expect too much hand
holding

on this what else yeah I figured

actually I would put it to the
audience

what it is that you would prefer to
see

and do do like remember that you know
if

you say ah we'd like to see a new

substrate module being done or I'd
like

to see how you develop the UI or how I

would see something else it is gonna
be

me sort of bashing away on a keyboard

for 85% at the time and if you don't

have a keyboard to bash away with

in-concert then it's it's going to be

pretty boring but feel free to to ask
me

to demonstrate or sort of educate on

what substrate does and how it works
and

I'll try and use my now freshly
compiled

substrate code base to help

help explain any so any any requests
no

requests yep

Adrian I certainly car would be my

greatest pleasure to you to walk
through

and anything beyond walking through

JavaScript's sorry walking through

substrate models in JavaScript yeah
sure

okay

sure yep

that's gonna be a month or two before
we

we've already done over that but yeah

you can expect that in the new year
it's

good that Rob's not here otherwise you

might meet cringing right now sure
yeah

okay I guess there's a few things
there

to be working on okay let's start with

the the last question because that's
the

one I can remember the easiest so if
we

if we had to depart the code base is

this can you actually read that or

should i zoom in a bit better or some

more yeah okay cause like the more i

zoom the less the less easier for me

it's gonna be boring it can also make

more space are a horizontal space yeah

okay cool yeah so a you TXO model it's

an interesting sort of one and one
that

we have just about begun prototyping
in

parity we've got a team of like two or

three people working on it basically
the

bitcoin style you 

# UTXO model 
is kind of an interesting hybrid between the

stripped/stricked all full nodes must validate

everything and and base their
validation

they the things that they validate
sort

of they put a state route/root a storage

route if you like state route into
each

block header and that means that like

clients when they come along they can

actually in principle validate

everything all of the state transition

logic of every block based purely upon

the state route of the last block
right

so if you trust that the header of the

last block is something or other then

you can check that state route and you

can come you can be provided with a

proof that all of the transactions on

the current block where executed

correctly and therefore that the state

route on the current block is is
correct

and polka dot very much uses this
model

it's the model that the power chains
are

based around and it basically means
that

validators by having just like client

logic can actually validate every
power

chain equally

I'm bitcoins a little different in

principle if you trust the previous

blocks header you can still be
provided

with a proof that the current block is

valid but the proof is potentially all

of the Bitcoin blockchain and
therefore

not super concise this it's this is
the

case because Bitcoin doesn't keep a

track of the current state at every

block and place that in the header as
a

cryptographic hash rather when you
want

to so make some transaction on Bitcoin

and prove that it's a valid
transaction

you use unspent transaction outputs
and

those are outputs that it is expected

that nodes will record in their
database

but if you want the actual proof that

those outputs are valid you have to go

all the way back to the block where
they

were they were the outputs from a
spend

in order to be sure that that they're

legit and obviously you have to see
the

entire chain between now and then in

order to be sure that they weren't
spent

already so it's it's much harder to do
a

you TXO model in literally the same
vein

as Bitcoin because this kind of light

client sort of model doesn't doesn't

quite extend to the conciseness
theorems

does that said if you instead store
the

unspent transaction outputs in a
murkily

structure in the same way that
aetherium

stores its contract state then you are

able to basically get a hash of that

structure every block and you can you

can provide concise proofs that a

particular transaction is valid
because

you can see the you can demonstrate
that

those and spent transaction outputs

appear in this localized structure and

that's how we would implement a u TXO

style chain within the para chain
model

and probably how we would implement it

in substrate and that's actually going

forward how its

we built in principle though as I said

that you could introduce a raw
database

into substrate and actually mimic

bitcoins model but it would preclude

turning it into a para chain without

adding some additional layers of

indirection and likely are not likely

enough trust which we're not so keen
on

introducing programmatically speaking

you'd introduce a UT EXO's at if we go

into the code base so I mentioned
before

like substrate has three levels of
kind

of you can dive into substrate of
three

levels so you can dive into at the
core

which is right there oh and this is if

you use substrate you use substrate
core

and it's got all of the things that
you

would expect to see in a in a block

chain if we look at the modules or the

crates as Arista falls them then we
you

know you can see things like the

consensus and the finality and the

keystore and the networking and the
our

PCs and so forth

it would likely be implemented as a

outside of the SRM L so the SRM L is

here because the SRM L is rather

designed around a an account and an

index model similar to aetherium in

principle you could nonetheless create

some additional SRM l modules that
kind

of introduce you to EXO's I mean in

principle as modules of
turing-complete

they've got entry point functions that

can be arbitrary bodies and they've
got

they can they can do basically
anything

with storage that they want so there's

no reason that you couldn't implement
it

that way but it'd probably be a bit
more

efficient and make a bit more sense

if you implement it as its own runtime

and to do it as its own runtime you

would basically just

use the underlying abstractions in
core

that were used in order to pass in
order

to allow things like the execute block

function to be to be implemented and
for

it to be called into by the the rest
of

the client and and work with that

directly and you define your own block

format and your own transaction format

and in general build the execute block

function and that's in Bitcoin from
from

scratch where as I say parity is is

pushing forward with its with a you
TXO

chain mostly so partly to make sure
that

the model that we have is actually

general it's it's a good sort of
sanity

check and partly because we actually

want to introduce a payment based
system

that's that's a sorry a payment system

that's based around you TX owes and

potentially that has some sort of ZK

snark functionality in as well and
we're

doing that with the by reusing as much

of the PBGC block parity Bitcoin and

code bases become cool I'm not going
to

start implementing one here am i okay

maybe laters cool

what were the other three questions

theorem compatibility yeah so how does

the RPC RPC and JavaScript work this
is

a yeah this is an interesting question

so substrates meant to be pretty
generic

and if we go to the RBC module it's

split into four four sub sub modules

author chain state and system author
is

to do with block authoring and it

contains have a look what it contains

submit

extrinsic there's a couple formats you

can submit them in and a Ana pub/sub
to

let you watch what's happening with

those extrinsic s-- so if you actually

want to inter inter oh if you want to

communicate with substrate through the

AH feces then these are the RPC is

you're going to use and it's basically

like here's an extrinsic it's already

signed it's already done and dusted
just

submit it broadcast it whatever and
let

me know how it goes and you from this

you'll get back various messages I
think

I know they were on the have a look

maybe I've got some messages yeah I

don't

you'll get various messages to tell
you

that it's been validated that it's
been

broadcast that or that it's about to
be

broadcast that it's been broadcast and

that it's been finalized or if it

doesn't get finalized if something
else

gets finalized in its place and that

it's now been dumped then you'll be

again told that it's been okay so back

to substrate 101 yes right to make to

make substrate be as general as
possible

we didn't want to introduce data into

the block header that may not be in

every block chain that substrate that

need not be in every block chain that

substrate is gonna cater for rather we

want to make the header strictly the

information that is required for

substrate and the rest of the rest of

the information that might otherwise
go

into a header in normal block chains
for

example the timestamp instead is

provided under a separate model that's

outside of the header called the

extrinsic and extrinsic just mean that

it's like data that is extrinsic to
the

block chain so it's not it's not

intrinsic to the chain

it's not like part of its state or

anything like that it's not a previous

transaction or anything that it can
draw

upon directly but rather it's just
data

from the external world is extrinsic
to

the chain and so data from the
external

world is provided in this set of of

snippets of pieces and each piece is

called extra and extrinsic and it's
just

short for a piece of extra piece of
data

extrinsic to the chain so right so

specifically the transactions that are

coming in coming in from the external

world they are indeed extrinsic

so most block chains possibly all

require there they're sort of

transactions to be signed like they
have

a particular signature scheme and if

they're not signed or if the
signatures

wrong then they'll never be considered

they won't even make it into the
system

substrates a bit different so
substrate

says extrinsic scan there are
perfectly

valid extrinsic s-- that are not
signed

they're extrinsic the only thing
special

about them is that they're a bit of
data

and they come with some way of

determining their length upfront so
you

you know you can pass them around on
the

network without worrying how long they

are other than that they're they're
just

arbitrary bits of data they may be

signed they may include a signature
they

may include a multi-sig they may not
it

doesn't really matter some pieces of

data for example the timestamp so

there's literal okay I can think of
very

easy Taric ways how you might possibly

argue that a timestamp could be signed

and that would make it valid like
maybe

if the the what's at the atoll the
guys

are on the atomic clocks like maybe

signed it with one of the keys but
still

someone could steal the keys and then

the signature it doesn't make it

specifically valid nothing can make a

timestamp specifically valid right
it's

a piece of data from the external
world

and the only thing that means a time

star

is correct as if we all agree that
it's

correct what time is it right now well
I

can say well it's 38 minutes past 6:00

in the evening but it's if your watch

says that it's 37 minutes past 6:00
then

you know there is no way how without

like finding a shared Authority
there's

no way how we can argue that one is
more

correct than another and certainly

signing it isn't going to help all
three

authors just an arbitrary name that we

call when we say author we just mean

block author so this is a block

authoring API this is an a this is an

RPC that you use when you want to

interact with the bit of substrate
that

is authoring blocks and this is
actually

incorrectly named it's very good that

you brought it up it shouldn't be
called

extrinsic s-- or at least it's it's
it's

a little over general to call the

extrinsic the only extrinsic that you

can submit using this API are

specifically signed extrinsic because

it's coming from the RPC and therefore

you have no other way other than
looking

for a signature to know that it's

anything that's really sensible
unsigned

extrinsic switch for what it's worth
we

call inherence because they are we
treat

them as being inherently valid are are

generated within substrate so
extrinsic

s-- is information that come from the

outside of substrate and inherent are

our extrinsic that come from within

substrate and sorry transactions
really

come from the outside inherent come
from

with them they're both forms of

extrinsic because they're both forms
of

data that can be introduced to the

blockchain and that might make some

sense to the runtime now if you don't
if

you don't sign some data then it's
like

well

you know how does that how does that

possibly work you know data's not
signed

then it could be anything right and
the

way that it works is the validators so

the guys that basically say that this

block is valid are the ones that get
to

be opinionated over whether this data
is

sensible or not and that's actually
how

this works in other blockchains as
well

so if you look to aetherium if the
time

stamp is actually just an opinion
right

a validator when they validate an

aetherium block have an opinion over

whether this time stamp is sensible or

not and if it's not sensible they
ignore

the block they unilaterally decide to

ignore the block and in in that sense

substrate is equivalent in its
treatment

of inherent if a is up to a validator

when they're proposing the block to
put

in sensible inherence and if they
don't

put in sensible inherence if there's

inherent in correct or incorrect in
the

opinion of the other validators then

their block will be considered useless

and won't get any further attention so

it's up to them to put in sensible

values for the inherent extrinsic that

are required by the block and this

basically means that we can do all
sorts

of interesting stuff aside from time

stamps I mean time stamps are the
first

use but we can do things like for

example with para chained candidates
for

the polkadot relay chain there are

that's another form of inherence why

because which candidates you choose in

order to in order to to be to be

therefore finalizing is necessarily an

opinion right it's different
validators

might choose different para chained

candidate blocks to be finalized for

that particular relay chain block

there's no way of deterministic elite

oozing the correct set of these guys

it's just whichever ones happen to
come

come by you but if you put forward

blocks that the other validators on
the

relay chain think are invalid perhaps

pick

they don't have the Associated data

because there's no data availability
for

them or just perhaps because they're

empty perhaps because they they just

haven't seen them before whatever
reason

it is if they don't think that your

opinion is is a good one over the

selection of these blocks and they're

free to ignore you in reality you know

they'd have to be quite malevolent

before they do ignore valid blocks

because they get paid basically for

passing these things but by and large

it's up to the validators to
self-police

the content of the inherent
transactions

the the sorry the inherent extrinsic
s--

for the transaction extrinsic s--
there

they are per se valid or invalid like

you can check whether a transaction is

valid by virtue of does it have a good

signature and whatever account has

signed it does it have enough funds at

this point in order to pay for the the

cost for it to be placed on chain and
if

those two answers are yes then it's
all

good and that's the difference between

the two so this is the RPC for how you

can submit them unlike aetherium we

don't provide any account management

stuff in here so what you can't do is

provide is try and get it to sign in
and

to sign a transaction all of the
signing

happens external to the node so the
node

is minimal

we do very well noticed the keystore

package is there specifically for for

block authors so far the validator
nodes

just stored their their block
authoring

keys so basically when you when you
set

up a node as a validator node which so
I

got away without doing this on the on

the demo that I gave earlier by using

the - - dev option which is shorthand

for - - chain equals dev - - validator
-

- key Alice but I could have typed
those

in and it would have done the same
thing

and so when you pass it the key it
will

it will use that it will basically put

that key into the the key store and
then

if it notices a key in the keystore is

one of the validators and it's set to

being a validator it will actually
start

signing what and and join in with the

consensus so that's the key store is

really just there for the for managing

validators keys for consensus yeah I
can

see yeah so there'll be some shims or

middleware in order to manage some of

this some of these are pcs to make to

make it compatible now I'll go into
I'll

go because this is kind of an

interesting exploration I think so
there

are a few other modules chain does

pretty much what you'd expect it to do

it's it's about blocks and headers it

lets you get transactions and as I
have

been chatting to a meal over the last

few weeks and months the the point of
a

substrate node isn't to provide a

generic sort of very generalized

database for every all of your uses
that

you might have I mean etherium was
kind

of pushing in that direction we
provided

all sorts of lookups that we didn't

really have to substrate keeps it

minimal it's it's got a fairly you
know

that's one of its Maxim's of design
and

one of the ways that we keep it so one

of the ways that this sort of minimal

RPC comes out is the fact that you
can't

look up specific transaction hashes
you

can I think maybe get oh yes so you
can

get a block and the block will come
with

a bunch of extrinsic sand in those

extrinsic will be the transactions and

that's that's basically if you want to

index if you want to have a you know
get

so get a transaction by its hash

you're gonna have to go to a chain

Explorer or a

party service provider or whatever to

get that now you can verify that that

that's the case just by getting the

block and transaction index and then

going back to your node and using this

RPC to get that block and then lookup

that index in the extrinsic and you'll

find your transaction or not but we're

not going to do that indexing for you

because it's too much work it's it's

better done on a dedicated software
yeah

sure

i-i'm sure or are all of the author
and

other extrinsic s-- you mean the
entire

chains worth

if the blocks not created yet then you

can't look it up at all when it's when

the blocks being executed no no no so

either the block has has made its way

into the database it's finalized then

it's good or you can't you can't read
it

at all during what sorry execution of

the block or well when a block
executes

I can show you that that that code but

basically there's an execute block

function and it gets given you get
given

a bunch of bytes and it's up to you to

execute those bytes so at the lowest

level so at core a core level yes SR
ml

it's a lot easier but that that's full

of a lot of opinionation that isn't

necessarily indicative of your chain

that you're writing on substrate

yeah yeah you just get given a bunch
of

bytes and that's your block and it's
up

to you to interpret the look of the

block as you as you desire okay let me

let me go through the are pcs and I'll

come back to you so yeah we can get

header block block hash and there's
also

a runtime version that should actually

be in state but it was a very late

edition Thank You Tomic Thank You
Tomic

we have system which just gives you a

bunch of information about the system

just the name version and the chain

that's basically just the the current

running node for example we also have

the JavaScript implementation of much
of

substrate and that will return
different

things from from these are PCs finally

as the state this is the most

interesting thing so again back in if
we

if we think about what you can query
of

a block of a blockchain state in other

block chains usually it's like account

balances whether an output is is spent

or not how much is sitting there

potentially some state of a contract
you

might even be able to make a call into
a

contract and have it execute we're a
bit

lower level with substrate as as might

be imagined from the fact this is a

library with a framework with which
you

build block chains but there are
things

that you can get from the current
state

of the chain are storage so we have at

the moment this RPC reflects a single

storage database so basically we have
a

merkel Merkel tree there is of
arbitrary

size keys and Val

in pairs right so the tree itself
stores

basically any key value any set of key

value pairs and the root is stored as

the state root and that goes into
every

block as you change the storage as we

change these keys and values then you

can that that will obviously change
the

state and you can make it's very handy

for building pair of chains because
you

can make very clear like client proofs

that one particular block with one
state

given these transactions that executed

or these extrinsic sigh should say
that

executed left another particular block

with another particular State

that's nothing nothing no really not

really any different to how etherium

works the main difference is that of

generalization and with our road map

that allow where we want to build

multiple different cryptographic

databases so if we if we think of the

Merc alized

key value storage that we have here as
a

one example of a cryptographic
database

then we want to actually extend this
to

other cryptographic databases and also

add a a means of having many of them
at

the same time so if you're familiar
with

how aetherium works there is a every

smart contract actually has its own
sort

of cryptographic database where it

stores its key value pairs it's
storage

if you like and and then there's one

sort of big one that all of these
other

the by virtue of them having state

routes all of these other databases
hang

off substrate only has one but it's
one

very general one and in principle
though

that can now be expanded to have as
many

as as is desired and they can even be
of

different types and they can be

dynamically added and removed in the

similar way to that you would

dynamically add and remove them in a

theory and when smart contracts go in

and out of existence

we also have metadata which is a

wonderful wonderful RPC that I will

spend some time talking about when I

address one of the other questions
which

was the color JavaScript

sort of magically works and finally we

have a pub sub for storage so you can

subscribe to particular storage items

and you can get notifications when
those

storage items change so if you

understand how storage works and
that's

one of the other magical things about

metadata that's what it kind of
provides

that's what it partly provides then
you

can basically keep an eye on that

storage and just be notified when that

entry changes yeah so Sergei can go
into

how smart contracts so this this
little

symposium will be will be split so
I'll

answer whatever sort of more general

questions about substrate and then

Sergey is going to do a little sort of

walkthrough of deploying a smart

contracting because explain some of
the

technicals behind the smart contracts

and we can go into the events and and

smart contract storage there this is

specifically for substrate chains as a

whole and this will be very likely
look

like the final RPC an API for para

chains so with smart contract so we
have

a smart contract module and that
exists

as a module within substrates SR ml so

you can basically make a substrate
chain

that can can manage smart contracts as

well as do a bunch of other things
like

have governance and all the rest of it

so we we kind of see smart contracts
as

a piece of functionality that you can

answer your chain or not add to your

chain as you see fit the

on your use case some use cases that

require a generalized sort of smart

contract environment some a very sort
of

specific fixed-function chains and if

their specific fixed function chains

then like for example a plasma chain a

plasma chain doesn't require a smart

contract modules and these smart

contracts in there right it's a fixed

function chain that would that
basically

manages all of the other chains
hanging

off it and what to do if those chains

want to do an early termination or or

settle on the upper layer so I don't

imagine all chains to have smart

contracts from finality in fact I

suspect many chain even perhaps a

majority of chains won't have smart

contract functionality

there'll be they'll settle for having

fixed function fixed pipe fixed block

processing functionality that's

upgradeable we like up readability
cool

so that's that's basically the our pcs

now the interesting thing is how these

interact with the JavaScript
environment

so we're developing because we like to

give people Choice we're developing

multiple JavaScript API is to interact

with substrate and substrate modules

now these api's are primarily geared

towards interacting with the substrate

runtime module library the SRM l so
they

are fairly their highest level right

there are lower level components like

for example you can use these pub subs

with promises I believe at the

JavaScript level but if you want to
but

we make it easy for you to use to
build

applications based around custom
modules

that you code in the SRM so you got a

taste of that although it might have

sort of flown past you a little bit in

the demo because that was trying to
get

it all finished and more or less on
time

but I don't know if you noticed but
when

I wrote that that little sort of toy
the

I was able to interact with the toy in

the browser as soon as the chain had

been upgraded I actually in the
console

got like an object that that let me do

it in like that was specific to them
the

code that I'd literally just written
in

rusty now I that wasn't a trick I
didn't

have anything up my sleeves it was it

was literally that cool what happened

was the runtime got upgraded although
I

didn't code any new JavaScript stuff

what the the code that I wrote and

annoyingly I can't really I don't have

know let me see if I can find that
code

should be in substrate node template
and

if I remember correctly I might even

have to I do okay so so if we go back
to

the substrate node template so this is

this is the sort of demo node that I

that I forked and started coding and I

have this demo module and if you

remember I did something like this I

said I did post sender and then
decoded

this this little address and then also

for the call

it's calls demo dot set payment and
that

just worked despite me not actually

having changed anything in the

JavaScript

I didn't even reload the page I think
I

certainly shouldn't have had to what

happened was when I coded this module

with play and set payment and when I

coded this storage with the two
entries

here payment and pot part of this
macro

generates some metadata about what
these

functions are and what these storage

items are and this includes the types

this metadata is compiled into a sort
of

blob and then amalgamated with all the

other metadata from all the other

modules by virtue of this macro here

construct runtime that you might have

remembered I put the demo line in this

then gets passed to through this macro

which implements the api's into as a

something that can be called into from

the client so we have here one
specific

trait that we can implement called

metadata as a metadata trait if we

implement it it means that metadata
RPC

will work and what it will do is it
will

check to make sure it's actually

implemented call into it which means

dispatching into the newly compiled

runtime that's been compiled into

webassembly

and interpreting this metadata and all

this this this function does is it
grabs

the metadata from the runtime and that

was caught that was built from this

constructor and time and that was
built

from all of the individual modules
which

are each are built from their

declarations of what they're stripped

their storage are and what their what

their module is that in terms of their

function entry points and this gets

passed into the JavaScript and as part

of the substrate JavaScript libraries
we

have a couple of libraries that you
can

choose to use one is based on

observables one is based on the bomb's

framework that I did last year and
these

libraries will in interpret this

metadata blob and turn into real

JavaScript utility objects that
reflect

and

[Music]

and will dispatch to and utilize all
of

the all of the module specific stuff
so

if we go to the way they put it

no just live demo yeah so if we go to

demo then it will actually interpret

will basically make Marshall all the

arguments and dispatch it through the

transaction dispatch system into yeah

and similarly if we query what the pot

is it will work out where this runtime

given this this module stores the pot
in

its storage and then go into storage
and

check that using these storage are PCs

and if you part of bonds is the fact

that it's reactive so if you tie that

into a reactive component if you

remember at the end of the demo I very

quickly did a little label that that

tracked the pot going up and down what

it's doing there is it's setting up a

pub sub with the thing that it figured

out all on its own with the storage

location for pot and track the value
it

knew what the type was because it
looked

that to see that this pot type is T

balanced and decoded it into a
balanced

type and then put that into a label

formatted it correctly and put it on
the

screen and it did all of that without
me

really having to do anything other
than

implement it right here in rust so the

idea with this is to make it not just

painless but actually fun to code

modules and the U is four modules what

was one of the other that was another

question mempool yes this is an

interesting one so we rewrote the
meant

well

Tomek I should say rewrote the mempool

recently we call it the transaction
pool

same thing

we wanted something that was general
but

also easy to use and it took a little

bit of thought but what we came up
with

was a a pool that basically allows

runtimes to specify dependency graphs

arbitrary dependency graphs of

transactions easily we wanted
something

that would work equally well between
UT

EXO model and a account and index
model

so basically a transaction pool that
was

equally viable to be used for
something

like Bitcoin and aetherium with the
idea

being that if we made something
general

enough for both of them it would

probably be pretty general
sufficiently

at least for doing interesting other

chains in the future so the way it

basically works is the runtime gets

queried and I can show you where this

comes in the runtime gets queried

through this validate transaction as

part of a transaction Q trait so this
is

a trait that you have to implement in

order for this transaction queue to
work

transaction pool actually just

references the executive here so the

executive is like a sort of
overarching

SRM L module that looks after just

executing a block in general in

particular it runs through each of the

extrinsic and dispatches them off into

the module that is expecting to that
the

extrinsic is sort of trying to call
into

all extrinsic in the SRM L framework

have a they they may or may not have a

sender they may or may not have a

signature they may or may not have an

index they may or may not have an
error

we actually allow we actually force or

not force we actually facilitate

extrinsic soar transactions to limit
the

number of blocks that they're valid
for

inherit

right so you can make an extrinsic the

transaction I should say that is valid

for the 16 blocks between a thousand
and

twenty four and a thousand and forty

right and it after a block a thousand

and forty it's just no longer valid it

can never possibly be make its way
into

a block chain this is needed for a
bunch

of other things particularly for
having

dust collection which I can go into

after after I've talked about this if

that's of interesting the transaction

the way that the validate transaction

thing works the important thing I want

to show you is the inputs and the
output

so the inputs are the transaction fair

enough

the outputs are the trend is this

transaction validity object let's see
if

I can find where this is defined this
is

basically a means of we see so we can

see it's got like three potential
states

invalid valid or unknown l invalid oh
no

no and a pretty obvious what they mean

valid so for a valid transaction we
have

a bunch of we have four sort of fields

that we have to define priority so how

what's this transactions priority

amongst peers for which everything
else

is the same

right so assuming this that everything

else is is all other things being
equal

which one should we favor to put into
a

block for in aetherium style chain
this

would basically be what's the gas
price

if the gas price is higher then we
just

favorite more we also have requires
and

provide so requires is and we have
this

vector of transaction tags transaction

tags are just Veck vu8 so in rust lack

of UI it's basically just an arbitrary

bunch of bytes right it's just a by
the

string yeah

and these tanks they're therefore

entirely up to the it's entirely up to

the runtime how it what tags it uses
it

can tag them by hashes it could
tighten

them by integers it really doesn't

matter could tag it by account IDs it

could you know concatenate a bunch of

these things and that could be the tag

they're entirely arbitrary all that a

transaction list transaction validity

function has to state is what does it

what tags does it require and what
tanks

does it provide now we can imagine for
a

for an aetherium style chain it would

require the tags that requires are all

of the tags actually let's start with

the tax that it provides tags that

provides our one tag that's formed by

the concatenation of the account ID
the

sender ID and the current index the

index of the transaction right the
nonce

yeah so you pop those two together

that's your tag right that's just an

identifier for the the thing that this

transaction provides what it allows us

to do is to then state what other

transactions require in those terms so

suppose you have a my transaction is

sent from me my account dev and it's
got

an index of 10 right so for that to
work

in the etherium model there has to be
a


sent by me one with index 0 1 with
index


yeah and that would make 10 to be the

next valid transaction so what it

requires is all of those tags now
that's

a bit unwieldy for when we get into
the

thousands of or tens of thousands of

transactions coming from a single

account so what we say is only
requires

the tags that have not yet made their

way onto the chain yeah and if if we
get

into the silly numbers like hundreds

then we just say either unknown or

invalid and we just say look this this

transaction is just way ahead of

schedule send it back sometime

future and that that actually is more
or

less the way that the etherium the the

parity etherium transaction queue sort

of worked just not in these terms now
if

you're on a UT EXO chain then it's

actually quite a bit easier because a
UT

EXO model like basically you've got
what

the tags are they're tags are just

transaction output hashes yeah so your

tags are it requires these unspent

transactions or unspent transaction

outputs so those transactions that are

providing those outputs are
specifically

the things that are required and it

provides well it provides its own hash

its transaction hash so you can very

quickly build a very easily build a

dependency graph from aut EXO chain
that

fits the Maps quite fairly well on to

this and we also have a longevity so

longevity is basically it's valid now

but for how many blocks will it
continue

to be valid in the future 4-bit Connor

and aetherium that's infinity
basically

transactions don't become invalid just

because time has passed but for for
our

substrate chains we have this notion
as

I said before of errors and the
ability

to inherently make a make a an
extrinsic

or a transaction in they're inherently

invalid merely after the passing of
time

specifically blocks and that's what
this

sort of lets the transaction so yeah

that's pretty much how transactions
work

in substrate this is more for the SR
ml

it is meant to be general so in theory

it can be used perfectly easily from
the

core from if you want to build like
your

own from scratch substrate chain in

principle this can be used fine but

you're also free

to write your own like there's there's

nothing you can you can plug in your
own

cue if you want to write your own cue
if

you have something that this model
can't

can't manage to embed the nice thing

about this though is that it goes to
the

runtime for all of its rules which
means

if you want to update your transaction

formats and you want to introduce all

sorts of new and crazy interesting
stuff

as long as you can put it in terms of

priority requires provides and
longevity

then you can implement whatever it is

that you want and when you upgrade the

chain all of the full nodes all of the

authoring nodes will all automatically

use your new transaction semantics and

they'll the transaction queues will

function accordingly yep

the only you can't modify so can you

modify a transaction in the that's in

the queue so what you can do is you
can

if a transaction has made its way into

the queue because it said hey it's
valid

and you want another transaction to be

able to to beat that one to it you
know

if the to a mutually one can you can

make it mutually exclusive by by
simply

giving it the same tag but higher

priority that will that will get the

transaction kids are sort of boot it
out

the transaction queue only feeds

transactions into the block if you
just

want a transaction to just become

invalid before it's been finalized or

before it's been before it makes its
way

into a block you can do that just as

easily by having a sort of you know if

in the execute block function if one
of

the transactions is this transaction

then invalid block right and what will

happen is the the unlucky guy who's

using this transaction queue to to
make

a block will will find that adding
this

transaction makes their block invalid

and

and it's kind of bad form to do it
that

way you should try and like make it as

much of a part of the queue as
possible

but if for whatever reason you can't
do

that then yeah you can make it invalid

just by virtue of changing the runtime
I

get in the office

Tomic can probably offer a little bit

more insight into this basically

priority is the is the first thing
that

it will select on and beyond that so
for

the transactions that have not yet

become valid then basically it's just

randomly choose randomly discarding

one's not that random is is
particularly

great but it also minimizes any

potential issues regarding censorship
so

if you've got like tons of nodes on
the

network and you really try to prevent

some particular transaction from
getting

through and you're doing that just by

spamming transaction out onto the

transactions out onto the network we

minimize the the worst case by by

choosing transactions at random to

discard because it's likely that

eventually one node will eventually
will

mine that that that transaction that

ultimately if lots and lots of people

want to transact at the same time then

there will be congestion so it's it's

really just the case of randomly

choosing transactions that come
through

I mean in principle priority here is

that's precisely what priority is for

times of congestion the runtime can

basically state which transactions

should be favored above which others
but

if if priority isn't enough or for

whatever reason maybe everyone's got
the

same gas price I don't know whatever
or

you don't introduce any particularly

compelling priority prioritization

mechanism then random is is the best
we

can do

cool Oh what was the what was the
thing

after transaction

I've forgotten no is there something

else to discuss yep okay anything
before

smart contracts yeah ah yeah okay yeah
I

can go into that so so the runtime

encodes a lot a lot about what what
the

blockchain does how it works

it doesn't encode everything in the

substrate model because we don't want

some things to be to be special what
we

could do is literally just put a rest

executable on the chain and when that

changes you have to kind of download
the

new one and run it but we don't like

that there are I think I may well need

to be corrected here but I think there

are there is a certain other project

that's trying to do governance and

upgrade ability and that's more or
less

the model that it takes so it sort of

basically puts its executable on the

chain and it's up to the nodes to kind

of download the new executable and run

that I don't like that model I prefer
to

be a little bit more conservative
about

the specific component that can be

upgraded and when we accept that part
of

the logic doesn't sit on chain for

example the peer-to-peer networking

library I don't think it's sensible to

put that logic on the chain and
there's

even questions about putting consensus

logic on the chain it could be that if

consensus logic is on the chain then
you

have this kind of monoculture almost

monocultural strategy for the
consensus

and that may lead to

attack vectors by virtue of knowing
that

all of the nodes use the same strategy

so if we're happy with not putting all

of our logic onto the chain then the

question is given that we need to have

the same data structures ostensibly on

the chain like in the runtime that's

upgradeable and off the chain in the

sort of general node itself how do we

how do we manage the fact that
sometimes

those structures will be those data

structures like transactions for
example

could be changed over the course of a

chains life how do we how do we like

manage the fact that these
transactions

have to then be sent they have to be

stored in a transaction queue and they

have to be sent over the network there

have there has to be some way of

handling these things right so we need

some sort of object and that's where

these opaque objects come in we need
to

a block needs to have some

characteristics and there has to be
some

object that we can call a block but we

don't need to go into the specific

meaning of a block beyond basic the

basic semantics of here's how to read
a

block number here's how to read the

parent hash and these these very basic

types or what we call the opaque types

and they are the fundamental types of
a

block chain that don't change from

Genesis onwards the in principle
they're

basically fixed now you say oh well
hold

on the extrinsic format is fixed and
the

block format is fixed and you know
then

surely you can't upgrade and it's like

well actually it that's the reason
that

they are opaque is because most of the

data that they contain is not stored
in

terms of specific fields like the the

real meaning

but rather it's just a blob a generic

blob and it's only the bits of the
data

that we actually need to know for

example we need to know the length of

the transaction yep the extrinsic we
you

know because if we're going to send it

around we need to how long it is and
so

what we do is we if we're going to

concatenate it together into a block

then we need to know how long each

particular piece of the concatenation
is

so we we say write the one bit of a

transaction of an extrinsic
transactions

format is going to be the length at
the

prefixed length but everything beyond

that we're going to leave undefined as

to its meaning we're just going to
tell

you how to decode and encode it you

decode it by reading the length and
then

reading that many other bytes I'm not

going to tell you what those bytes
mean

but that's how you decoded it and you

encode it by seeing how long it is and

then putting the length at the
beginning

and then following up with the other

things by avoiding defining this stuff

we can then define it in the runtime
and

then as runtimes upgrade we're not
left

with incompatible data structures that

for handling these things in the
client

now it's important that so this isn't

something that we do with every type

it's just the fundamental cut types
that

clients do tend to have to be able to

handle basically because we need to
pass

these types around the network or

because we have to interpret them for

the consensus algorithm so block
headers

one type extrinsic

one of the type and I think hashes may

be although hashes are paint data
anyway

I think it's basically just those two

types yeah yeah although again

signatures are opaque data anyway

specific so you've got two types of

signatures that float around in

substrate you have to consent there
are

signatures on the consensus side and

signatures on the signed on the

transaction side so signatures on the

transaction side they don't need to be

the same and the runtime entirely

interprets those I can well imagine

extra

substrate chains that don't have any

signing any cryptographic signing on

them at all that they're they're just

you know the validators choose which

things come along they you know from

maybe some trusted corporate
centralized

database and they're just funneling
this

data in and it's going into the chain

and just being recorded and maybe

there's some state transitions that

happen on the chain but basically it's

that that's there's no need for

signatures the consensus side yes
there

are signatures but I guess the
consensus

algorithm to define their own

cryptography anyway I mentioned what

sorry yeah so I I kind of spent a
while

describing that but I mean I can I
guess

I can probably if I can launch a node
do

I have one already oh yeah sure

okay so yeah I have a node running
okay

so this is I think the demo node I'm

gonna upgrade it okay so

yeah okay so is that too small I guess

that's way too small right how do i is

that still too small it's too big okay

so this is template node V - which I

think means the demo is in but I can I

can check this by so I mentioned there

was the run time object fun time okay
so

the run time has a bunch of these are

the various modules in the run time

right so balances consensus core demo

that's the one I just coded earlier

today system timestamp upgrade key and

version these as I mentioned come from

the metadata so I can I can show you
the

code I mean it's it's I don't know how

interesting is if we go into the

double-oh-seven

if we go into the double-oh-seven

library and then the double-oh-seven

substrate and then the bonds this is

where they get created let me see if I

can make this a bit bigger okay

can you read that just about

yeah I need a I need the drum player

okay so these are these are the sort
of

easy ones that just they're just sort
of

hard-coded height header head fash
name

version and chain I mean you saw the
RPC

end of these we have this node service

that just ensures there's a single

WebSockets connection to the node so

it's like a singleton and then we can
I

assure a request and these these are

these are single shot requests some of

these like head there subscriptions so

there they use the pub sub
functionality

we have runtime version so I can eat I

can I can give you a lighter quick and

runtime I think it was in the core and

then console log and that's what comes

back very simple got the api's this
this

exactly represents this structure in

rust that is in runtime template yeah
so

this this represents exactly this

structure if I change this structure
and

upgrade the upgrade the chain then
this

will automatically update if I run
this

this thing again it will it will give
me

the new values yeah and it's the meta

it's actually it's not the metadata
that

encodes that I think but that is that
is

fetched in this from the runtime in
the

same way that the metadata is fetched
so

the meta metadata is built
automatically

per module in the runtime amalgamated

together by virtue of a couple of
macros

and then pass the line into the

JavaScript and in the JavaScript we
have

a so we actually have a serialization

library very very lightweight
basically

just memory representation
serialization

library and we have the opposite in

JavaScript so we can serialize and

deserialize in rest and we have the
same

thing in JavaScript and we can kind of
a

bit like a bit like JSON or whatever

except binary and sort of designed to
be

very fast and very minimal lightweight

yeah that's yeah the you mean these

modules yeah so these are indeed there

is a web pack running somewhere yeah

yeah

so got like MPM sitting around in the

background updating are checking for

changes and packing it all together ok

so there's there's two things going on

so the first is the these module these

UI components those you have to code

independently so they don't come from

rust they're not automatically made
you

actually if you want to add a new a
new

module you have to sort of get get
yeah

typing fingers out and and and go into

wherever it was that I went into now

here we go yeah where is it now here
so

this is the yeah this is the UI and
this

isn't special at all this is just

standard reacts semantic UI with web

pack and NPM packing things in there

looking out and watching it and
packing

it in the background so there's
nothing

I don't I don't so if I if I add any

module then I have to press controller

yeah but what happens I didn't press

control I just now to get sorry if I
do

a blockchain upgrade I don't I don't

have to press ctrl R now because the

metadata automatically gets updated in

the background so that doesn't come
from

the thing I was mentioning earlier
with

the hard-coded things so the bonds
these

bonds here the version bond and the
the

name chain the hi header hash these
are

all hard-coded they don't change their

fundamental are PCs that substrate

provides but the other things the
things

like if I go to run time and like I

don't know balances if I want to get
the

balance of an account or accounts do I

have let's decode an account let's

decode this account so I want to get
the

balance of this guy then I can decode

that into an account ID and give it to

balance and then print the output and

it's going to tell me that this guy

doesn't have any money I think yeah

balanced zero and then if I if I gave

this guy some money so let's send some

funds to him or her it's a hex address

I'm not sure if they have gender do I

have any Alice has got some I guess

Alice is a girl and I'm going to send
I

don't know if I send a thousand units

that's going through sent and then if
I

go back to balance is then we go now

it's got a thousand yeah so this stuff

when we go into specific run time

modules they're not built they're not

they don't come in by virtue of this

code this code builds them

programmatically from the metadata

is has a pub/sub relationship with the

with the node itself so as the nodes

runtime gets upgraded this finds out

about the upgrade grabs the metadata
and

rebuilds all of those objects and data

structures and letters now interact
with

with the new runtime in a organic

fashion but if we want to add UI

components then we have to go in and
do

them ourselves and press controller
the

real a chain what what does it use

so polka-dot uses power to codec

polka-dot uses basically all of

substrate like there's not much in

substrate smart contracts is the only

thing it doesn't use and yeah other
than

that it looks pretty similar to our

basic substrate node so it's got all
of

the sort of functionality in apart
from

a smart contract but it does have an

extra module that's specific to polka

dot called

para chains module so I haven't I'm
not

like it's been a little while since I

looked at the para Chains module but

that's something that will become
pretty

important

over the next POC so PRC 3 was
dedicated

mainly to our new consensus algorithm

that would be the final our algorithm

for polka dot shaft

or grandpa I don't know you know I
mean

I do know obviously shaft but some

people don't not everyone agrees yeah

sure so shaft is a actually is al here

so al al invented shaft at the time it

was called afge Al's finality gadget
but

we're kind of renaming it and
basically

it's a progressive adaptive
progressive

finality consensus algorithm designed

specifically for polkadots use case
and

the idea is that it's a it adapts to

network situations and validator

pluralities if you have lots of

validators sorry if you have few

validators and a really good network

then shaft will finalize as quickly as

instant an instant finality algorithm

like tender mint or rhododendron which

is our sort of take on pbft for the

blockchain but even if you have slower

Network and lots of validators it will

finalize as fast as it can behind the

scenes and the way it does that is by

having a transitive relationship over

the blocks so the way that pbft
derived

algorithms work in general is you have
a

candidate block so every every block

number that you want to finalize every

block that you want to finalize there
is

a candidate right it's not yet
finalized

so it's just one of many and then you

put it to a bunch of vault authorities

or validators that the participants in

this consensus game and they will each

if it's a pbft based thing they'll do
a

a pre commit

to basically say this is the one that
we

think should win and then if they see

enough other pre-commit they'll
they'll

move on to a commit and there were
rules

about how you whether you can change

from a pre given that you've pre

committed on one thing whether you can

change your you know what what you're

allowed to commit to and if you've

committed on one thing whether you're

allowed to switch that commitment and

these rules basically guarantee that

assuming no more than two thirds plus

one Byzantine nodes or Byzantine

participants in the game that you will

always come to an agreement on what it

is that on a particular candidate now

this is fine it's a perfectly
reasonable

way to go but the problem is that
block

it's not optimal because block chains

have relationships blocks have

relationships between each of them so

blocks that are all of the same number

so they're all at the same level
they're

just can different candidates for the

same potential slot don't have much of
a

relationship beyond being mutually

exclusive but blocks that appear
before

so parents or ancestor blocks have a

very very strong relationship right

cryptographic relationship we store

their hash which means as an
implication

that if you voted for block I don't
know


that that 68 references by virtue of
its

parent hash you're you've also voted
for

that right you've also voted for 66
or


Genesis block assuming that the
network

conditions are such that you are not

going to be able to afford to finalize

every block then a vote for 68 should

definitely imply a vote for 64 67 and
so

forth but for that 67 yep so if
there's

two folks if there's a fork and what

it's made its way to block deep

okay maybe asn't made yeah actually
you

could have in principle have made it's

way too much deep just at the
production

stage then it's important that any
vote


not allowed to do under this
transitive

model is both commit to 68 on one fork

and to the 67 on the other fork and
that

that's one of the things that now

becomes illegal but because that's

illegal it what it means is that we
can

interpret a vote for 68 as a vote for

all of the blocks that haven't yet
been

finalized on that on that fork and if

suppose we suppose where that last the

most recently finalized block is

terrible network conditions the most

recent recent finalized block is like

say block 50 yeah so we're running 18

blocks in advance of the of the most

recent sort of had agreed on head of
the

chain then if we've seen let's suppose

we have 10 validators and they're

basically just validate they're just

voting for the blocks that they see we

can only get out one vote per block
and

it's our vote so actually that's not

I've got 20 validators and we can only

get out one vote per block what would

happen is that under Undershaft these

votes we would always be running at
the


actually no because we only need 2/3
so

it'd be the 13th or 14th most recent

vote vote because we can accept all of

the votes that are that are were on

blocks after let's say where were 50
so

suppose we're trying to finalize like
I

don't know 53 yeah all of the votes on

that fork after 53 so every validator
is

like doing one vote per block so one
of

them is voted on 54 one of them is
voted

on 55 one

voted on 56 all the way up to 67 or 68

where every were then it's likely that

every new block 2/3 plus 1 will have

voted on one more than the last block
in

total access account ensures

every new block that's being produced

that validator will be voting on that

new block which will imply they voted
on

all of the blocks up until that new

block which will imply you've got one

more vote assuming a constant cycling
of

validators one more vote on that chain

in general and so because that chains

been brought forward by a block
because

everyone's voting on that chain then
the

the the the most recent block to be

finalized will probably be a bit
further

along as a network conditions improve
we

can get invalidated can get more

messages out then that will catch up

naturally because the greater number
of

votes will be recent and and therefore

again transitive relationships being

what they are you'll be able to

basically form a consensus that two

thirds plus one validators agree that
in

fact 62 is the most recent shared
block

that they can all finalize it's a pain

to spot when a validator has behaved

badly basically because the rules are

transitive sorry yeah rules are

transitive and so you have to enter
into

this in the worst case enter into this

game of querying well hold on you you
as

a validator voted for this block on
this

chain but yet you voted for that block

on that chain why what were you seeing

what what signatures did you see that

made you switch chains and they can

either equivocate and say move don't

know because they actually they switch

chains in

invalidly or they can provide you with

the signatures in which case you have
to

go to the next guy along and say well

you signed for this chain but you you

know and the game continues basically

it's quite a an unwieldy game and it's
a

it's a bit of a pain it's a bit
tedious

to play but it's it's something that
can

happen on chain and it's not the end
of

the world beyond that it's it's got
some

very nice proofs about how live it
stays

in an asynchronous environment and its

optimal over the it uses the notion of

block transitivity optimally in

determining under a pbft style
mechanism

which is the most recent lock that it

can be considered final okay

under optimal Network conditions

assuming the validators are well

connected with each other and that
their

plurality because I mean it's pbft so

you can't get around this N squared

message passing stuff requirements but

assuming that they're well connected
and

and there aren't too many of them then

you know I don't I mean you know this
is

where empirical measurements come in
but

certainly with our test net so far

there's no reason to think that we
can't

handle

final final is finalizing most blocks
so

not staying very far behind the block

production head at all so the block

production is handled by a separate

consensus mechanism that doesn't
provide

finality at the moment we're moving

towards or and which is a randomized

shuffled version of hora which
basically

just sounds for Authority round and
you

just go around a bunch of round-robin

style block production game but it's
our

intention eventually to move to
probably

something similar to a ruborous which
is

a it's basically like proof of
authority

mapped on a proof-of-work mapped onto
a

proof of authority schema so all
thority

get given lucky slots whereby they can

mine a block and and it's a way of

generating generating these lucky
slots

and and I'm ministering them in such a

way that that you get given a bunch of

guarantees about Byzantine tolerance

so Network network topology or

peer-to-peer networking in general
we're

using lib p2p which is pretty modular
I

would in principle there's nothing
that

would stop you from writing a new
module

that would provide the sort of

networking that yeah I mean if you
want

something like Onion Routing on like

mixed nuts yeah I don't see why that

wouldn't be possible using the
existing

network module system worst case it's

probably like implementing it as a as
a

new API but that's something we'd look

into now I think I think it's what you

can can build on top of it I mean we'd

certainly be interested to sort of

explore if the API is a sufficient in

order to provide that but I suspect
they

might be okay more questions when how

long it was at 8 p.m. do we want to

maybe you think yeah yeah I it will be
a

little bit harsh but yeah I think ok

let's let's erm maybe move the
questions

towards the end and do a I don't know

some exciting stuff do you want to
plug

in the HDMI oh yeah I'm glad that
you're

doing this cuz I need to go to toilet

yeah yeah sure thing use do we do we

have working toilets yeah yeah

outside so gay oh cool

so let's get started this demo is like

well it wasn't prepared at all because

of his ins and I and I had to
improvise

so yeah um yeah let's get started so

because I basically don't have any UI

because of as I mentioned reasons I
have

to do like I have to craft in
extrinsic

by hand

basically yeah but I have some little

code snippets that actually can help
me

with that so it's not that problematic

is it okay yeah

okay so let's start this let's start

with a fresh chain for that that make

that maybe might take some time

yep yep it's basically the branch a

second I know I would say now because

yeah for some reason I chose to use
the

soft shoe type oh yeah so let's clear

the DB and then let's launch this new

chain yeah so it seems to be working
and

yeah so so to make an extrinsic we
need

some to do some boilerplate code and
for

that we need a genetics hash so like
it

it is required to create extreme six

that that can be executed on this

particular change so we need to first

fetch the block hash of the Ganesha's

and for that I will I'm going to use

the RPC cash so I will call block hash

function this argument zero which
which

is for Genesis block and so oh yeah

actually it's already there yeah and

after that I will regenerate all
extreme

six so yeah so this extrinsic this big

blob of up our data is contains

instructions to deploy a certain

contract and the the most of this data

actually represents webassembly smart

contract and yeah let's let's actually

submit that extrinsic up have this one

but it doesn't yeah oh yeah

yep so to verify that it actually

executed well let's see the substrate
UI

oh do you have an idea how to look up
or

a nonce of Ellis yeah yeah yeah

as I said network I can connect to you

ah yeah it's def okay yeah in
JavaScript

oh sorry sorry guys I can't stop I
come

on okay so the runtime and then
balances

is where the hold on it's not system
by

the index is stored and then I can't

nonce and then which I can't ah

Ellis Oh Alice and we should get
Alice's

so we need to introduce a the seed for

Alice which is a pain to do if you
don't

do you have some key I yep and try to

just execute it in here yeah

it's okay yeah it just works

oh but is it the latest version of

something it's from air each hour but

I'm not sure yes later

yeah does work cool totally I expect

okay

oh how can I help you I want to get

these Safari oh okay

right so we put that scene in there
you

put it yeah so while this has this
yeah

now this is a short oh yeah this is

something that should go over actually

because this is kind of cool but I'll
do

it afterwards so we can try that

everyone can see what's happened there

right okay

Oh doesn't suffice all that well

I wouldn't be surprised actually okay

[Music]

maybe it's not about ten part I mean

maybe yeah nons account okay

maybe should open fire

let's just check decode and then
Alice's

ID okay so that's done something that

wasn't a type error but it's so that

means that our transaction actually
got

in and we deployed the test contract
so

now I'm gonna show what kind of
contract

this is yeah so so this contract is

basic counter which has actually two

functions to action that we can
actually

execute on it let me do ya so the
first

one is in command that takes you 32

parameter which specifies by the value

by which we should document this
counter

and get function which basically
returns

the value and when the contract is

executed we just read input data it's

basically the same as in Hulme and

decode it into this action

in a very enumeration and then
depending

on what actual variant of this you
know

we performing the action and this in

case of increment we just query the

contract storage by certain key which
is


the specified value and then save it

back into the storage encoding it and

before that and get is basically just
we

read the storage and return that value

and that's it yeah

so this contract is already on on the

this chain and yeah so there is a

extrinsic that which with which we can

call this specific contract and it
takes

address of that contract and address
is

calculated by the constructor code of

this contract and the original address

which sent this which sent the create

extrinsic and this so again yeah

actually it's used and so it's
specified

right here so for the first
transaction

it was zero extrinsic and for the
second

one it's one yeah okay I see what you

mean for determining contract address

nonce is not used only code of

constructor a region and just like

input data and that's it yeah yeah

basically yes but you can alleviate it

with providing different data so we
push

this issue to user side yeah

and yes so we doing call this to the

contract at the specified address this

we thinking some funds to it and also

specifying how much gas we want to

allocate to that and also we're

providing the input data for the

contract and this data is actually
just

encoding the index of the action so in

in our case it's 0 so it corresponds
to

this variant and these four bytes

encodes basically number by which we

increment yeah

and let's let's execute this yeah for

now it's the only way but like we

basically don't have either cell right

now and it's the thing that is

developing right now yeah so yes

and so these extreme six extrinsic is

for calling this contract

yep so I think it is executed and to

verify that it's actually performing
the

the task let's just check the
substrate

storage for for this corresponding

storage key of that contract and I
have

because it's kind of hard to my to
know

which address is that I have a special

function that actually calculates this

for me and yeah and let's take it and

[Music]

it's really yeah so okay here's the

result

and it's corresponds to one in encoded

in little endian format and this is a

specific thing for encoding the length

of this data so yeah it basically
worked

and then let's try to increment it
once

again by I don't know

yeah wait a second I don't understand

that

so this extrinsic is in memory pool
and

let's wait until it actually yep it's

mind and then let's query the storage

and yeah it looks like it's eight yeah

sure yeah that's a good question let
me

show you know so basically it's more

more like a few Mike environment and
so

for now we basically have bare minimum

for writing contracts and it's I get

said storage and then call create
which

basically is very similar to thin ones

return checking input size and also
like

this it also have has a scratch buffer

and it's something that is not in
Hulme

so because of yeah I didn't mention
that

that storage in

substrate contracts have like
arbitrary

size at values so you can put not
only


you query it you you can basically

return it in this user specified area

I mean contracts it's fight area of

memory but then like it's like if the

contract doesn't track track the size
of

the values that it actually puts in in

this in storage then it could easily

overwrite the like memory of the

contract and this can be a bad thing
so

because of this we all arbitrary size

that values are put into scratch
buffer

and then contract can actually query

this buffer to actually load data from

it yeah so it depends actually so you

can't use I know anything that depends

on files trading and things like that

but like everything that doesn't
require

my saturating system things like files

and things like that and can be
compile

it to the assembly and then probably

fine

yeah so basically this system is very

similar to a theme and we basically
have

like a smattering very similar to a

theme I mean like every year Vice

Minister action is metrid and like I

mean every instruction in the block
just

Metford as the price of the dairy
prices

are similar up and just deducted from

some counter and if it goes to the
zero

then there's out of gas succession
yeah

so we are planning on bringing in

chromatic gas yeah yeah and also they

think I wanted to mention that we
about

the API that we are also thinking
about

giving the contractor on time

chance to call different modules
outside

of I mean inside the run time so it
can

actually call do Kevin and well yeah
so

means governance modules even the
power

chain module that would forward any

potentially could could have open an

entry point a function call to forward

messages into a para train so we can

imagine is a smart contract sitting on

one pair of chain sending out messages

to a smart contract on another pair of

chain or sending messages into a

governance system so a smart contract

chain like the edgeware chain that was

mentioned today could have smart

contracts that you know manage
people's

voting preferences yeah that's
something

I mean yeah for the gas it differs a

little from aetherium one of the
things

that I wanted to bring into a theorem

like when it was too late to bring it
in

was this notion of chromatic gas which

is basically having three different

kinds of gas one of them for
processing

one of them for memory usage and one
for

storage with the idea being that

basically we can compute maximum gas
per

block in terms of resource usage for

each of these three but doesn't make

sense that if someone gives up some

processing power that they can instead

bloat the chain by some storage right

then the two are not the two are not

somehow mutually exclusive so what we
do

is we actually have the resource usage

independently for each three and then

every contract gets basically whenever

you spend one gas on processing you
sort

of get one gas on the other two for
free

or vice versa

the gas sort of comes in in in white

light where you get all of the

independent gases sort of tied
together

and then you can use them as you as
you

want and what you don't use either
irem

really decided but it either gets sort

of ignored or goes back into the pot
so

that other things can in principle

overspend but what this does it

basically means you've got much better

resource usage utilization and you
don't

have the issue whereby you can bloat
the

chain by 3x what you should be able to

just by using all of your gas to enter

into storage and none of your gas or

memory or processing which is what the

sort of scenario we're stuck with

aetherium oh why not GPU as well well

maybe version 2

yeah we'll go into the contracts right

so yeah this so for like the gas
system

is only a concern of contract model
and

any other modules doesn't know about
it

at all yeah I think so

then cool so I'll mention one other

thing which is the account indexing

which is what we saw earlier do you
want

to know I can do it on yours I think I

mean yes but it's it's a nice little

thing so it's worth it's worth knowing

it well can you how do I get to Safari

okay so if you noticed before while I

was mucking around down here can I
make

this bigger somehow you can do this

little trick oh just so there is a if

you hold ctrl you can scroll it like

that yeah so if you notice up up here

Alice has a so I we've got two
accounts

here default and Alice default is this

big long big long address analysis
this

lovely little address right so this is
a

legit these are both legit addresses
and

if you you know in principle if if a

substrate chain gets listed on an

exchange then in principle you can get

one of these addresses this is just an

address like in Bitcoin in etherium

right but they both are so Alice this

isn't some cunning name registry or

something this is an address we

understand yeah we're going with this

[Music]

in principle yeah it will mean
different

things on different chains because it

will reference a different account
just

this just as the other one the long
one

would so the long one is a they're
both

based 58 representations they both got
a

checksum in them they both got a
version

byte in them the difference is that
the

first one is a 32 byte public key from

that represents a point on the edy 255
1


nine public key and the other one is
an

index and that's why it can be so
short

so every account in in a if you use
the

SRM L in substrate every account has a

an index associated with it I should
say

every nonzero balance account writes

every account that's active has an
index

associated with it if an account
doesn't

have doesn't have any balance then it

doesn't get one of these nice short
IDs

because there's no index I so it can't

like then you marry every account then

you may as well just use the 32 byte

identifier but if it's got something
in

it then you can use this use this
index

instead to identify it and we've got a

mechanism whereby if it's within the

first 200 or so then you get to just
use

one byte and if it's in the next sixty

or thousand then you get to use the
next

byte they're two bytes and I think

there's only like tens of thousands of

active accounts on of active accounts

that have any real significant funds
in

on like aetherium so i don't i suspect

on substrate chains we'll probably see

most addresses to be fewer basically
be

like four six five or six characters
and

it basically can look that up in its

enumeration of all addresses and and
as

these things become free so if an

account goes to zero substrates SRM
our

balances module will automatically
claim

reclaim that that address right so it

basically deletes everything to do
with

it

and this including this enumeration
and

so if someone like has one of the

earlier Guinea I can't imagine that

there's gonna be something like clamor

to get the first to enjoy the accounts

that have you know these four four

letter addresses cause like there's
only

so many of them and before long
they'll

go but if someone like reduces their

balance disease or to the minimum
below

the minimum allowed balance there's
this

notion of an existential deposit which

is basically the minimum amount of

balance you must have for your account

not just to be deleted dying like that

if they're iam Bitcoin don't really
have

this which leads to an awful lot of
dust

accounts that have like a none spend

ibly small amount of of ether in it so

small that you know you can't actually

send the transaction to but yet
they're

still recorded by the state so they're

still taking up a lot of space on disk

exchanges are particularly bad for
these

things but you know they they leave a

bit of gas on the spent and that just

that's just a tiny little bit of ether

just clogging up the train chain to
the

like 10,000 accounts a day sort of job

so this is why in principle yes so if

this account becomes a discount like

reduces below the existential deposit

and then someone else claims it then

that address won't last forever so

there's couple of ways of dealing with

that the first is that that address
has

in it a checksum and that checksum

refers to not that address itself but
to

the account that that address points
to

so if they were to if they were to do
it

maliciously it wouldn't happen

accidentally the checksum would look

after that they were to do it

maliciously they'd have to mine an

address that whose checksum was the
same

and these accounts these small ones
you

can choose which address you want to
use

yes sorry you can choose which address

format you want to use and some of the

address formats have like eight byte

checksums so they'd have to mine an

address that had like eight whole byte

that shared an eight byte

start of a hash with with the address

that sort with the address that's IO

that's that so I mean I can see this
not

being so so much of an issue and to be

honest if you want if you want an

address that's like you know permanent

then sure just use the big big long
one

yeah

but if if you want something that's
like

a payment address or if you absolutely

sure that you're never going to go
below

the minimum amount then as these small

addresses should be fine and they're a

lot easier to remember and the other

notable other point is that if we
delete

accounts and this came up in aetherium

so parity we apparently made a
proposal

on the IPS to sort of say look we
should

have disk election that was actually

implemented on covin right we rolled
it

out to covin and the issue one of the

issues is that it means that nan

basically transactions can be replayed

if you're not careful because when you

delete an address you don't just
delete

the balance and reduce that zero but
you

also delete the nonce which is
counting

how many transactions it's sent which

means the first transactions can now
be

replayed or the very first transaction

in particular can be replayed and then

the second third and fourth can if
that

goes through successfully and that's

problematic or it could be problematic

because if those transactions do

something other than spending the

balance then you can actually put

balance into the now zero account so

it's now got cash to spend

and then replay those transactions to
do

this to replay the side effects this
is

why we brought in the transaction
errors

so the idea is that when you construct
a

transaction it's only valid for a

particular period for the future so
you

can make a trend you can construct a

transaction that's valid indefinitely
we

allow for that but if you do that
you've

got to accept the fact that if someone

else if your account gets deleted then

the first transactions can be replayed

primarily because there may be well

firstly cause we you know we're

generalists and we like to you know

allow for as much as possible and it's

up to our users who we we believe our

our esteemed you know we esteem them
in

terms of their intelligence and we

believe that they know what they're

doing I mean you know everything goes

wrong at some point I I don't believe
in

making tools you know I don't believe
in

making kind of safety scissors for you

know for people I think that you know

the level at least that were at you
know

we should we should be working with

sharp knives for sure official and but
I

would put this on the user level so I

think this is for you know if you read

some of the other chains they issue
kind

of not warnings but directives to
their

middleware and use a user level like
UI

engineers say look this you can cut

yourself the chain doesn't protect
users

from doing the stupid thing so it's up

to you to protect them by putting in

these warnings and we will do the same

so for example if you're if you've

issued if your wallet knows you've

issued transaction resolve doesn't
know

that you haven't issued IRRI limited

transactions and sees that a
transaction

you're about to issue would remove
your

account with the would delete it would

take you below the minimum amount then

it will would basically bring up this

big what it would either not let you
do

it or bring up a big warning saying
are

you sure you know what you're doing
yeah

yeah so if you know we don't want to

like hamstring people at developing on

the platform we want to inform them
and

we want to inform them when they
should

hamstring the people who were using

their what they're developing or at

least give them information to make
sure

that they don't shoot themselves in
the

foot

yeah it's true I mean there's maybe a

trade-off to be made here checksum

seemed a perfectly reasonable thing I

mean most address formats have checked

sums in the original aetherium didn't

and that was a really big pain so that

was that was just less and learnt in

this case I don't think there has been
a

lesson yet to be learnt and I think
it's

such a really easy thing to protect

against like either wallets shouldn't

let you construct transactions without

errors that should be a niche thing
that

you have to build your own
applications

to do I mean the cambia that I can

imagine applications where errors
don't

fit for example suppose you've got

something like a plasma plasma kind of

chain and or a state a state channel
and

you need this transaction to
potentially

last indefinitely

because that's the only guarantee that

the other side has that you won't just

pull out after the era is ended yeah

it's like there's no other way of
doing

it and there you just have to ensure

that for your specific niche use case

there is no way of reducing that
balance

below the existential deposit but in

most use cases user level tools can
make

sure that you don't shoot yourself in

the foot say again

oh when you see well it's a
transaction

hold on so when you serialize a

transaction I see what you mean yes
can

we carry yes the answer is yes we can

carry our concise address format into

transactions as well so normally we

would have to put a 32 by identifier
for

each address for the sender and the

receiver irritatingly so one of the

things that if you read about Edie two

four five one nine they'll be like oh

yeah it's really good it's got like

compact signatures its signatures are

smaller there are only 64 bytes
instead

of 65 what they don't tell you is that

unlike unlike ECDSA you do need to put

the sender address in there so yes you

might you might shave a bite off in

terms of the signature but you have to

add an extra 32 bytes on because you

have to tell it who the sender is

I mean Bitcoin doesn't kind of Bitcoin

provides both but you know whatever

that was just a questionable decision
if

there him doesn't we go we don't
bother

encoding the sender in the transaction

because we derive it directly from the

signature you can't do that with EDD
si

sorry with Edie 2 5 1 9

so actually signatures are 96 bytes

we have a binary format so we don't
have

padding we have a binary format that

basically uses 33 bytes to encode one

other 32 byte addresses and uses one

byte to encode the small ones and and
so

forth and you can determine what it is

from the first few bits yeah so it did

if you use these small addresses you

shave off like 60 bytes potentially a

couple more from the transactions and

that's that's a pretty decent saving
it

basically cuts transactions by 2/5 as

the relay train yeah I mean so the

substrate comes from the relay chain
so

when we started coding polka dot well

when we started coding substrate what

has become substrate that was just

called polka dot and although even

before we started I had in my mind it

was going to turn into substrate I

didn't really mention that to the
other

devs so they were a little surprised

when I started renaming stuff but yeah

it's the biggest the biggest challenge

is kind of thing that I mentioned it's

getting that level of generalism while

still making things be easy too

while still making it easy to write
the

things that you want to be able to
write

so that that's what you know any
design

of language or API comes down to this

getting this trade-off right and it

ultimately comes down to expressivity

so expressivity of a of a language in
a

domain is really just how well you can

make it easy to do the things that you

want to do and make it possible to do

everything

yeah and bringing those two closer

together is is what we call
expressivity

or what we call a good language or a

good a good domain language

and you know substrate has a similar

challenge getting it getting it
general

is not so easy

it's something I've you know kind of

wanted to do even since CPP aetherium

but not having had all of the
experience

it wasn't so easy to do it the early

days but it gets it gets a bit easier

with having coded a couple more Sega
is

data deleted when it's deleted

okay so what was the question again

okay so specifically with smart

contracts or with runtime modules and

and the the substrate in general not

really but smart contracts kind of
have

to do their own thing to some degree
but

okay I'll talk about substrate in

general because that's what I can
answer

yes I see I know what you mean now I

know what you mean yes no contracts
work

differently

okay so we have we have archive mode

that keeps everything around forever
it

doesn't run an archive mode by by you
by

by default it runs in a standard
pruning

mode by default which basically Clips

any state from blocks that are old

enough after a 100 or 200 or so if
what

you're talking about is like in a
theory

and when a contract delete something
if

that contract if that call into that

contract turns out to be exceptional
and

everything gets rolled back then that

delete doesn't happen in substrate in

the basic runtime module in the basic

sort of core we don't do roll backs

right so as soon as something gets

written into storage or deleted from

storage it's actually deleted and
unless

the entire block gets rolled back

because it panics then it's done that

thing's deleted and there's no way you

can you don't have checkpointing in

there if you want checkpointing which

you may well want it goes into the it

goes into the as a runtime module

library and the contract runtime
module

library that the smart contract
actually

implements checkpointing so it's it

implements something that is basically
a

minimal form of aetherium but has most

of the stuff of the etherium
environment

and the only difference is that it

doesn't run in a VM it runs in

webassembly

of course web assemblies kind of cool

and sexy unfashionable and hipster

yeah I mean you know to some degree

runtime upgrades and these sort of

out-of-band

state changes are are specifically
what

we do how we manage what happens when

things go south in other ways so

ultimately there's the same safeguard
if

you want to call it that that any
other

chain would have that doesn't have a

governance system which is it can
always

be hard forked even if the governess

system of the polka dot substrate

whatever goes Ori I I sort of take a

two-prong approach and say well on the

one hand we want to be reasonably

conservative and try to get solid

reviews of our proposed governance

mechanism and/or use governance

mechanisms that already exist in real

life and that we have some idea will

probably work okay with the other
prong

being try it out as much as possible
on

test nets that actually have value

before we put it on to a network that

has lots of value potentially and this

is you know where the edgeware project

could be very helpful and potentially

others like it to really provide a

network where you know no one's losing

anything if it's if it there's no cost

to it going down or being problematic
or

having to suffer a reversion but there

is real value at stake

because it's got tokens that are worth

something

and that's that I think is a really
nice

middle ground to sort of go and try a

bunch of stuff that we think probably

works okay but we can't be sure to

really try you know empirical testing

try it in real life in much the same
way

that when the psychologists want to
run

a new experiment you know they they
get

a bit of money and they sort of say
look

if you if you do this right you get

fifty quid and you know this is the

instructions offer you go kind of the

same thing and we did something
similar

with the Olympic test net back in PRC

nine PRC ten days for the theorem to
try

and get people to kick the tires of
what

would become the the main net before
we

released I actually I think Olympic
was

a not a failure but I don't think it
was

a great success not that many people

took part in it and it didn't really
it

didn't discover it didn't uncover one
or

two of the books that came up which
you

know we would have otherwise hoped and
I

think a test net that has real value

maybe less value than it would
otherwise

have but still real value is probably
a

much more market-oriented and
therefore

probably more successful way of of

trying to incentivize people to to
break

it or otherwise test it

yeah so I mean you know

etherium had the consensus tests it
had

a decent first testing although I will

note that when we implemented parity
we

found two points where the consensus

test didn't cover and we had to we had

to add new consensus test it's a hard

thing to do with with substrate we
kind

of work around it a little bit by
saying

there's a single was a reference on

chain so it's basically like a machine

executable reference spec I think it's

it's questionable whether that's such
a

great thing I mean the Bitcoin guys
love

that idea because it's like they've
got

their reference implementation Bitcoin

core I kind of like the yellow paper
for

not being machine executable but yeah
I

mean in polka dot and substrates case
it

was largely forced upon us anyway

in principle there could be multiple

implementations at the same runtime I

could you know that's something that I

explicitly provide for in the
versioning

I'm not sure if it will happen the

foundation the web sorry foundation is

looking to fine teams that yeah cool

okay yeah so I mean I'd like to see it

happen but as for whether as for
whether

we could really come out come up with

better consensus tests than what we
have

for theorem I'm not sure it's a really

tough thing to do without literally

paying someone to painstakingly go

through the specification every single

conditional write a test for and I
mean

this gets potential exponentially

complex as the different modules I
mean

you're looking at kind of two to the

number of conditions potential number
of

tests that would have to cover

everything it's it's crazy so I would

probably just go for

taking out a month's rental of a

supercomputer and running a huge
amount

of first test I think with a with a

well-designed for sir that that should

probably provide as much protection as

as you would otherwise get in polkadot

so there's there is a mechanism sperm

free transactions basically for

transactions under the substrate model

which polkadot adheres to transactions

must sorry under the substrate the SR
ml

model which polka dot adheres to I

should say transactions must pay for

their space on the chain so there's a

fixed fee for a transaction and
there's

always I mean can set it to zero if
you

want but you know in principle there's
a

fixed fee that a chain can leverage
and

this is a something that can be
changed

and upgraded and all the rest of its
if

it's the parameter and there's also a

per byte fee that's charged per byte
of

the transaction and as it's encoded on

the chain so that's non negligible

that's sorry that's non-negotiable
they

have to be paid beyond that there is
an

if you want to charge additional fees

for what the transaction is doing then

that's up to the chain and the runtime

modules and the sort of the the code

that you're writing yourself you can

charge for what you want polka dot
will

probably have some yeah fees
associated

with the creation of balances with the

occasion of accounts so the balances

module has a fee schedule that you can

again customize if you've depending on

you know how much you want to charge
for

the sorts of things of creating new

accounts reaping all the counts

transferring balances as a bunch of

things that you can

customize what else other modules I

think most modules actually don't

specify much of a much of a fee
schedule

because most of the modules are yeah

there are the governance modules do
have

a fairly complex system of deposits

that's mainly to address sort of a
game

theoretic concern of a potential

quadratic quadratic attacks where

quadratic cost attacks where certain

algorithms are order N squared to rent

or order MN to run and you have to
make

sure that you know m and n can't both

get too big it's ok if one of them
gets

kind of big but you need to make sure

that they can't both get too big

otherwise you end up sort of going off

off into the distance in terms of the

cost and that's managed through as I
say

a set of deposits and basically

incrementally increasing the cost of
one

or the other depending on how big the

other one is I think that's probably

about it I'm not so worried about

literally charging every single clock
of

processing like gas would do partly

because a lot of a lot of operations
are

order one and/or they have like a

reasonable maximum that is not that
you

know they don't really go proportional

the user can't force or the trans
actor

can't force a anything over a
particular

maximum that maximum is relatively and

significant and I would look upon it

more or less as the Bitcoin chain does

where it's fine

basically to just charge for space on

the chain for the size of the

transaction and not think too much
about

the processing costs because they will

be dwarfed by the storage costs

obviously in smart contract land it's

much more important to charge for the

for everything because you know users

can potentially write infinite loops
and

all the rest of it so their contracts

module does indeed make sure that

there's a proper costing mechanism for

them

it's not a final design decision to be

honest but part of the reason is that
it

makes a lot of sense when block

production is on a individual basis
like

- so - are competing with each other
to

produce these blocks it's not really
the

case in the consensus are mechanisms
or

block production mechanisms that I see

substrate working under you in in

substrates case that's really more of
a

sort of collective thing and it
doesn't

really make sense to reward one over

another and partly it's I guess
remember

the other reason I think there was

so in some sense when you burn fees
it's

a little like paying all of the other

participants I guess yeah okay so the

reason that that miners collected fees

in etherium was because that's what
they

did in Bitcoin and the reason that -

collected fees in Bitcoin was because

eventually the block reward was going
to

go down to zero and that would be the

only thing that incentivized - to do

anything that's not the case in in

polka-dot in poker ah the the
validators

are are incentivized like permanently

through the reward system it basically

means though dilution will happen
there

they're sort of percent or their ratio

of their state compared to everyone
else

will remain the same they're sort of

dilution protected and everyone else

gets everyone who's not part of the

validation nomination sort of system

will get diluted and because that's in

place it doesn't make so much sense to

then

reward them extra for for the

transaction fees that said it's not a

final decision as I said and I'm like

you know something to talk about over

the next few months and see if there's
a

compelling reason yeah I mean

potentially I'm not sure we really see

that in in real life there's plenty of

instances where you know aetherium

transactions go through four zero gas

price censorship you know as always
can

always happen you know it's just if

you're going out of your way to censor

some transactions there's a cost

associated that selection mechanism
and

that cost may be insignificant but
that

cost may be the price you're willing
to

pay maybe and it may be substantially

greater than the cost of you know not

collecting a couple of micro sense of

fees so for sure but if they do then

adding a small transaction fee isn't

really going to make much difference

unless unless you know the
transactions

that they're dealing with our micro

transactions in which case

yeah yeah I mean that's it so staking
is

more of a sort of collective pursuit

rather than something cutthroat yeah I

got a feeling there was another reason

but I can't remember what it was now
but

as I say it's not something that I'm

happy for this to be a continuing

conversation and you know if it turns

out that it does make sense for the

block producer to to take these fees

then this point is just the simplest

code like I don't it's one line of
code

for you basically

suppose you have 20% of the chain that

is 20% of the total capital contained
on

the chain total tokens that is part of

the staking system and market the
market

effects are such that 5% of that 20%
so


the total are being replicated to root

for their rewards so I mean this is a

half the reward right 25 percent but

whatever then in order to prevent the

rich get richer situation of the chain

where validators just everyone gets

diluted and all of that dilution is

favoring the validators their
percentage

of the total state just keeps going up

and up and up and up until eventually

their total of the amount of the chain

approach is like 100 percent to avoid

that situation we place a well if we

save 20 to 80 then we place an
according

amount so for X in our 20 to 80 ratio
of

the amount of that reward goes into a

pot right so

so we've got 20% a quarter of that
gets

paid to the to the validators so we're

going to mint 25% of of the chains

tokens again yep so we're going to

dilute by 25% we take a fifth of that


so validators whatever so that's that


and with the other 20% which which

basically is 1/5 of 80

sorry 1/4 of 84 fits over 25 that gets

placed into a sort of community Dow
for

the chain not too dissimilar from you

know the thing on - or or even like
the

Dow although let's boogie hopefully
and

that will be used as like there's

already a treasury module in here it's

very simple but there's the treasury

module actually sort of put forward
ways

of spending and and they can be voted
on

and then the idea is to basically
allow

additional modules to spend money from

this pot and they can be sort of

integrated through chain upgrades it

could also be a lottery it can also be
a

reward for people that like accounts

that are willing to bet on the price

going up if you got an oracle feed of

the price so if you're willing to bet

that the price goes up and the price

does indeed go up after a period of
time

then then you can get paid from this
pot

and if it goes down then you can your

money gets put into the pot so there's

all sorts of interesting don't think

about that too hard there's all sorts
of

interesting oh yeah you can like what
I

showed you like the thing is what I
was

trying to think of a toy runtime
module

that would extol like that I could
code

live you know that wasn't so big that
I

couldn't code live but also that sort
of

got across

a degree of sort of generality that
this

provides and unfortunately everything

that you can code live pretty much
will

look like a smart contract because
smart

contracts are just tiny little bits of

code that you add to block chains but

what I did with with the extra module

although it looked a bit like a smart

contract it worked very differently

right this this was a deployment of
code

that would be run natively and that

would sit in principle in the same
place

as the smart contract as the smart

contract bit of the etherium state

transition function or the EVM or

whatever else you want to think about

code I won't even consider coding an
EVM

interpreter as a smart contract that

would just be zany right I'll just be

Cray would run so slow and it'd be

really horrible but you could do that
in

one of these modules because it
operates

at like a 10 X low lower level so you

get that 10 X in in performance and in

well it uses that it uses some
substrate

runtime module for its pyro change

management yeah I don't call it

delegation because that's kind of got
a

loaded meaning now that EOS uses it in

particular when you delegate a stake
you

don't get punished if the if the party

that you delegate to misbehaves

and that's usually problematic in
terms

of getting incentive correctness so we

call it nominating and they're
critical

as I say the critical difference is
that

when you nominate a validator with
your

stake they you lose your stake if they

misbehave I mean they lose those first

but if they don't have enough to pay
for

the misbehavior then you lose yours

yeah yeah so you this is to get around

the nothing at stake issue long term

long range attack yeah so yeah there's

it it's bonded the moment the
assumption

is it'll be about three months bonding

period but it will be at least as big
as

the expected period between releases
of

the of the binary of any software that

sort of interprets this chain so no
the

assumption is that the most recent

checkpoint will go into the software

itself and the bonding period is

sufficiently large that and the
software

will be released sufficiently
frequently

that the most recent bond most recent

software release will be holding
current

validators currently bonded validators

to account okay Sagan you can if you
put

it in yourself I use fixed point for

anything that needs point one more if

you're a move floating point then it's

deterministic cool okay I call an end
to

this are you do you want to do it you

wanna do the end
