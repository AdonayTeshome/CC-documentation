# Credit Commons documentation


This repo will give access to the core documentation for the Credit Commons Protocol project, to be implemented as a Gitlab Pages site.

This ReadMe is what we have at present.

## The Credit Commons Protocol

A protocol designed to allow accounting contexts of all kinds to be connected for transaction processing through federation (each context remaining sovereign).

An open protocol designed as a ledger transaction primitive for proposition, validation and completion of transactions between any two accounts on any ledger with a valid address in a namespace 'tree'.

The driving intent is to allow accounting contexts which are socially manageable (ie operate at some version of 'local' scale) to associate together in groups which are also socially manageable: this federation process produces an arbitrary, fractal 'tree' structure, which can deliver economic 'network effects' without requiring centralised governance (states/fiat currencies) or trustlessness (permissionless crypto).

The Protocol is defined as an API.

Each ledger in the tree is assumed to be an independent server - a 'black box' which is required only to implement the Protocol (respond appropriately to API calls) and have a valid address in the namespace.

The protocol, by design, requires only a mminimum of necessary information from each ledger in respect of its immediate 'trunkward' and 'leafward' connections to process a transaction.

A transaction fundamentally increases the balance in the account to be credited, and reduces the balance in the account to be debited appropriately ('exchange rates' set by each ledger may mean that the debit and credit numbers are not the same). 'Policy' and 'Business Logic' are allowed for, to be implemented by each ledger autonomously: these allow for a wide variety of specific characteristics at ledger and transaction level to be developed, so that the Protocol can support significant diversity of implementation without loss of transaction processing interoperability.


## Purpose of the Credit Commons Protocol

The Credit Commons Protocol allows any two accounts in an (arbitrary, fractal) nested tree of accounting ledgers to be appropriately adjusted (ie performance of an accounting transaction), reliably keeping all such transaction records in order. Transactions are 'validated' according to any policies which may have been applied through governance, but the Protocol does not 'care' what those policies are. The overall balance of any intermediary (non 'leaf') ledger will be unaffected by any transaction (by default the balance across all accounts on such a ledger will always be zero).

Transactions may also be labelled with a 'business logic' identifier - on the basis of which other transactions may be automatically generated. but again, the Protocol does not 'care' what these are, or why they have been generated.

There are a (very few) Protocol level administrative functions - notably the ability to run a transaction in reverse to effectively delete it.

What the meaning of the numbers in the accounts is, what the intent of the entities intitiating the transactions might be, what types of business logic may be applied - all this is irrelevant to the Protocol - assumed to be specified higher in the stack.

The intention is for the protocol to be usable in the widest practical variety of contexts, under the widest variety of conditions (of both technical implementation and social agreement).

This is to facilitate the invention and elaboration of the gamut of social relationships which are in any way improved by the maintenance of abstract accounts, while ensuring that it is technically possible for those social-relationship-contexts to connect to each other for exchange in some way improved by the movement of accounting numbers, if they consider such exchange useful.

The Credit Commons Protocol is designed to allow the development of a 'Credit Commons' - but is not itself such a thing - any more than 'the internet protocol' known as IP *is* 'the internet*.

A 'Credit Commons' describes a set of federated ledgers, forming a ' fractal tree', where the ledgers constituting the trunk, branches and twigs function as 'mutual credit' exchanges, while the leaves, by contrast, can be anything they wish to be - anything from a single person, to a gift-economy collective, to a football team, to a blockchain, to a business or group of businesses, to a megacorporation.

The only thing any of these needs to do to participate in the Credit Commons is to find a ledger which it wishes to be a member of, and which will accept it as a member.

Just as lone leaves can grow from mighty branches, any ledger can have 'leaf' accounts - in other words, members whose internal accounting method is private to them - and can be anything they like.


## Project Status

A Proof-of-concept Protocol and reference server implementation were created in 2020 (now deprecated). A beta release is under active development.

### Project traction
Two wider projects support the Credit Commons Protocol

   - The Credit Commons Society - a 'foundation-in-waiting' collaborative society working to promote adoption of the Credit Common Protocol , act as guardian for the open source and social purposes of the Protocol, and foster learning, research, training.
   - [Mutual Credit services](https://mutualcredit.services) - an 'as-a-service' business working in partnership with a variety of trading contexts globally to implenent specific ledger models appropriate to a wide avariety of contexts (all capable of intertrading via the Protocol).

## Help Wanted!

An open protocol with ambitions to become a significant primitive for thousands of ledgers (at the least!) needs 'many eyes' on it prior to being promoted for mass use.

### Developers
   - 'Black Hat' the protocol : it the protocol is  to be capable of global 'VISA-scale' transaction processing at the 'trunk' of a tree, it must be capablew of being implemented in and extraordinarily robust, reliable and speedy manner. Is this true? All sorts of work is necessary to build confidence on this point.
   - Reference implementation : currently written in PHP - can you help with this?
   - Reference client : needs building - can you help?

### Communicators
   - Improve repo structure : in order to attract 'many eyews', the project must be easy to engage with. Can you help with this?
   - Diagrams : better diagrams are needed. Can you help with this?

## Developer documentation (itself in development)

### Overview
The Credit Commons is a Protocol describing how mutual credit ledgers, arranged into a tree, can record transactions between any accounts on any ledger. It can be thought of as nested mutual credit accounting.

Mutual credit means, in this context, that there are no 'goods' on the ledger; it logs movement of accounting units representing credit/debt between accounts; consequently that the sum of all account balances is zero.

The tree is made with two types of connection, one between two ledgers, and the other between a ledger and a client, meaning a mobile app, or perhaps a social networking application  The API methods supported the creation of transactions, its 'movement' along a workflow path, retrieval of transaction data such as account balances and transaction listings, and a few others.

Thus every payment or transaction recorded is between two accounts on the same node. A transaction that spans, or traverses many nodes, is likewise always between two accounts on each node, where all accounts but thos at either end are mirrors of accounts on other nodes.

Every child node (leafwards node) has a special account called a balance of trade account which mirrors that nodes account in the parent node (trunkwards node).  This means when accounts in the leafwards node pay to the BoT account, the leafward node's account on the trunkwards node must simultaneously pay out. In effect the credit has traversed ledgers, leaving a record, or two records in the mirrored accounts. In this way it is possible to construct a tree of ledgers of arbitrary size, and for any account on the tree to pay any other.

This documentation describes everything in terms of a tree, but the simplest implementations would consist only of a single node and its user accounts as leaves.

### Core Concepts

#### Mirrored accounts and cryptographic ratchets.

It is critical that the accounts which 'connect' the ledgers be kept provably synchronised, or else the branch of the tree would be detached and become temporarily or permanently a separate tree, its leaves unable to make payments to the main tree. This is done quite simply by hashing certain fields of transactions in mirrored accounts, including the previous hash, and then it is possible to check the integrity of the 'connection' at any time by comparing the last hash for each account. For obvious reasons, the hash does not include a timestamp, account names which vary according to the ledger's position, and the amount is always expressed in the trunkward node's unit.

#### Relative and absolute account names

The tree structure works similar to a file system, even using the same / symbol Accounts can always be identified by an address starting at the root node, or relatively from the current node, by beginning the address at any branch closer than the trunk. Whenever a node encounters an address that doesn't begin with an account name it knows, it assumes that a trunkwards ledger will know what to do with it. If the account name begins with an account it knows and is more than one item long, the address belongs to a leafward node. 

#### Workflows

It would be too limiting if transactions were created in a single step. Many use cases require that the 2nd party to the transaction confirm it because it is finalised. Sometimes transactions may need to be erased, suspended, delayed or any number of other operations dreamed up by financial engineers. 

Each transaction has a type which refers to a workflow path it can follow moving between various states via transitions, which are performed by permitted accounts, relative to the transactions. This map of the possible states and transitions is a workflow. 

The API includes about 5 builtin states, but others can be added. The workflow defines the 'starting state' to which the transaction is first written.

Thus a 'cheque' transaction might be created by the payee into state 'pending', and then the payer might be allowed to 'sign' the transaction to move the state to 'complete'. Then perhaps an admin or the payee could 'shred' the transaction if desired.

When a transaction traverses multiple ledgers, all involved ledgers must be aware of the workflow in order to validate every state change. Nodes are expected to support all trunkward workflows and relay them to all leafward nodes. This ensures that workflows declared a the trunk are available to all the tree, but also allows more localised workflows.
@todo show a workflow object in javascript/yml?

#### Chaining queries

We are not aware of other multi-server-side architectures that need to chain requests and responses, so we have innovated here as well. Requests can be made to branchwards and leafwards nodes, so there is a need to use a different metaphor so as not to confuse with traversing up and down the tree. 

An http request is said to come from upstream and go downstream and conversely an http response comes from downstream and goes upstream.

A request from a twig which goes downstream and upstream is called a 'phase' e.g. the validation phase, the write phase.
Error and transaction validation failures are also affected. A node cannot simply return an error code or message as to a client, but messages must be relayed all the way back to the client which must also know which node originated the message.
For this purpose the API passes errors as objects, of two types, violations, which correspond to 4xx status codes (user or data input problems) and failures, which correspond to 5xx status codes (internal software problems).

A node which receives an error object from downstream should return the object upstream, untouched.

#### Node visibility and privacy

Each node is politically independent and assumed to participate freely in the tree. For the purpose of making transactions, nodes do not need to see the ledgers of other nodes except to know that the hashes of the mirror accounts match. An API call allows a leafward node to know its account limit on its trunkwards node, and hence to do some additional validation before propagating the transaction across the tree.

In principle though Leafward nodes may choose to expose ledgers or transaction summary data, as part of trust-building with their peers, who after all are also their sometime creditors and debtors.

Conversely trunkward nodes would normally expose their ledgers with all their leafward nodes, because the leaves are the members and hopefully the governors of the system, and they have an interest in the ledger integrity.

#### Account types

There are two types of account;

 - the leaf or local account, 
 - the remote account which is mirrored on another ledger.

End users connect to their leaf accounts using client software such as a mobile app or social network, and all transactions are between leaf accounts. This is because every remote account points to and synchronises with another node.
Each node might designate leaf accounts for administration purposes, and give it certain permissions, but that does not concern the API.

The library contains several classes for remote accounts in different positions (e.g. upstream, downstream, branchwards, trunkwards) because this can be useful to know how to represent the account address and the position of the node in the transversal transaction (e.g the trunk, a node on the way to the trunk, or a node going away from the trunk).

#### Transaction lifecycle

A transaction, or rather, each entry in a transaction, records a flow of credit  between two leaves. The transaction must be initiated on either the payer twig or the payee twig. The workflow corresponding to the transaction type determines whether it can be created by the payer, payee, or some other party, such as an admin. There is an API call from the leaf to the twig sending a simplified transaction object.

When the upstream twig receives this first transaction instruction from a leaf, it first converts it into a normal transaction object. That means it creates a UUID, sets the state to init, and separates out the header fields from the entry fields. At this point there can only be one entry, called the main entry.

The account names are parsed to determine whether they are local, branchwards, or leafwards, and from that, the node deduces its position in the transversal transaction. For example if the connected (leaf) account is local or branchwards and the other unknown, then the transaction must be relayed trunkwards. Or if the connected account is trunkwards and the other account local, then it should not be relayed at all.

Then the create/validate phase begins. The transaction is sent to the business logic microservice. This microservice has a single API method and can be written independently; it can be devastatingly simple, such as appending a 2% fee from the payer to the admin account, or it could use impenetrable algorithms. In any case the blogic service responds with one or more entries to be appended to the transaction.  

Then the node checks the transaction is valid. At the very least validation means that the workflow type is known to this node, and that the resulting balances of the local accounts would not exceed the limits for each account.

If the transaction fails validation, the node can return a response object immediately, but if it succeeds, then the transaction is relayed downstream where subsequent nodes may add entries and validate it against their criteria.
What comes back from downstream is only the additional entries because the rest of the transaction is immutable. The transaction at this point is written to a temporary space. 

The create/validate phase is now over and the workflow determines what happens next. Either the write phase begins immediately and the instruction is relayed downstream, or more likely the transaction is shown to the original user with its additions for their approval. In the latter case the upstream twig returns the full transaction to the client including a list of permitted transitions, which for a 'validated' transaction is one, 'confirm', which means write the transaction to its starting state.

If and when the user hits confirm the leaf sends only the uuid and the desired state name to the twig, and then the write phase begins. The same request goes downstream and the transaction is written when confirmation comes back that it is written. The transaction has now been written into its starting 'first' state (determined by the workflow) on all relevant ledgers.

Whenever the transaction is served to a leaf it includes a list of permitted transitions which can be rendered into buttons. A state change consists of a validate/write phase, validation being needed because most state changes will affect users' balance and hence account limits. That means a user might be permitted by the workflow to 'erase' a transaction in principle, but after pressing the button the validation could fail and the operation be denied. 

#### The transaction object
 - universal transaction ID, created by the upstream twig.
 - version number (the number of state changes in this transaction's history.)
 - state: the current workflow state.
 - type: the workflow path
 - entries: an array, of entry objects each one being
	Payer: absolute or relative path of a leaf account
	Payee: absolute or relative path of a leaf account
	Amount: number of units on the trunkwards ledger
	Description: free text
	Author: the local account from which this entry originated.
	Metadata: an arbitrary object 

####  Imperfections
As it stands the API has loopholes which would cause problems in high traffic or mission critical situations. These mostly seem like hard problems which may not be solved in version 1. These problems will all have been solved in other systems and it expected that with expert help, solutions will be found.

#### Rounding errors
Tiny decimal errors between nodes won't cause accounting problems, and should cancel each other out in the long run. However they could cause problems if the amount of each transaction is included in the mirror accounts' hash function. To prevent this the calculation is always done by the leafwards node and the communicated amount in both directions is denominated in the unit of the trunkwards node. Even so it will probably be necessary for the Protocol to specify the number of decimal place OR for it to allow the trunkwards node to determine the number of decimal places.

#### Order of transactions

The ratchet depends on each of the mirror accounts putting the transactions or to be more precise the transaction transitions written to the ledger, in the same order. This could be difficult because they each initiate operations independently of each other, meaning that operations would inevitably be initiated simultaneously. Maybe this can be solved by making a rule, say, that the trunkward node determines the order of writing whether the transaction is going upstream or downstream. Since web servers mostly handle requests independently, being multithreaded, this might necessitate some kind of request/response buffer shared between threads.

#### Nodes offline
If a node goes offline or worse, a ratchet breaks, then the system may be able to partially cope by accepting some transactions and absorbing the risk that they wouldn't clear. New API calls might be needed to catch up with backlog, or to undo the last transaction which broke the ratchet. 
Also what if a node goes offline midway through a transaction write phase?


#HUGO CONTENT

Info relevant to the deployment of this site on gitlab Pages using the Hugo static site generator.
![Build Status](https://gitlab.com/pages/hugo/badges/master/build.svg)

---

Example [Hugo] website using GitLab Pages.

Learn more about GitLab Pages at https://pages.gitlab.io and the official
documentation https://docs.gitlab.com/ce/user/project/pages/.

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [GitLab CI](#gitlab-ci)
- [Building locally](#building-locally)
- [GitLab User or Group Pages](#gitlab-user-or-group-pages)
- [Did you fork this project?](#did-you-fork-this-project)
- [Troubleshooting](#troubleshooting)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## GitLab CI

This project's static Pages are built by [GitLab CI][ci], following the steps
defined in [`.gitlab-ci.yml`](.gitlab-ci.yml).

## Building locally

To work locally with this project, you'll have to follow the steps below:

1. Fork, clone or download this project
1. [Install][] Hugo
1. Preview your project: `hugo server`
1. Add content
1. Generate the website: `hugo` (optional)

Read more at Hugo's [documentation][].

### Preview your site

If you clone or download this project to your local computer and run `hugo server`,
your site can be accessed under `localhost:1313/hugo/`.

The theme used is adapted from http://themes.gohugo.io/beautifulhugo/.

## GitLab User or Group Pages

To use this project as your user/group website, you will need one additional
step: just rename your project to `namespace.gitlab.io`, where `namespace` is
your `username` or `groupname`. This can be done by navigating to your
project's **Settings**.

You'll need to configure your site too: change this line
in your `config.toml`, from `"https://pages.gitlab.io/hugo/"` to `baseurl = "https://namespace.gitlab.io"`.
Proceed equally if you are using a [custom domain][post]: `baseurl = "http(s)://example.com"`.

Read more about [user/group Pages][userpages] and [project Pages][projpages].

## Did you fork this project?

If you forked this project for your own use, please go to your project's
**Settings** and remove the forking relationship, which won't be necessary
unless you want to contribute back to the upstream project.

## Troubleshooting

1. CSS is missing! That means two things:

    Either that you have wrongly set up the CSS URL in your templates, or
    your static generator has a configuration option that needs to be explicitly
    set in order to serve static assets under a relative URL.

[ci]: https://about.gitlab.com/gitlab-ci/
[hugo]: https://gohugo.io
[install]: https://gohugo.io/overview/installing/
[documentation]: https://gohugo.io/overview/introduction/
[userpages]: http://doc.gitlab.com/ee/pages/README.html#user-or-group-pages
[projpages]: http://doc.gitlab.com/ee/pages/README.html#project-pages
[post]: https://about.gitlab.com/2016/04/07/gitlab-pages-setup/#custom-domains
