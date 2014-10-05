---
layout: post
title: How does the Bitcoin blockchain work?
date: 2014-02-08 17:39:45 +0100
comments: true
published: false
categories: 
---
The 'blockchain' technology underlying Bitcoin is touted as a 
novel technology that makes many new ventures possible that were previously 
impossible. I wanted to understand what exactly this 'blockchain' is and what 
can be done with it. This post documents what I found.

# The point of Bitcoin
The point of Bitcoin, as stated in the original [paper](https://bitcoin.org/bitcoin.pdf) 
by Satoshi Nakamoto, is 
> to provide a peer to peer version of electronic cash [that] would allow online
> payments to be sent directly from one party to another without going through a
> financial institution

# Challenges for Bitcoin
The main challenges to be able to achieve this, apart from the bootstrapping
problem addressed later, are: 

* ensuring that the payer can prove he paid the payee
* ensuring that a payment cannot be reversed by that payer or anyone else
* ensuring that a payment is recognized by all relevant parties (basically: 
  everyone that you may want to pay at a later time)
* ensuring that the same funds cannot be spent twice
* ensuring that this all works in the face of untrusted and malicious peers

Bitcoin achieves these goals by:

* collecting all transactions in a public ledger, shared and agreed upon by
  all clients in the network, that contains all transactions in an order
  that cannot be changed after the fact
* transactions use [public key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) 
  to generate unique addresses and unforgeably (and irrevocably) signing 
  transactions from those addresses with the private key
* generating new bitcoins to compensate those that spend computational resources
  towards the security of the public ledger

# Some phraseology
* Wallet: a collection of addresses used to execute transactions
* Address: a unique number, derived from a public key, that you can give to 
  someone to send you bitcoins or to identify that you send them bitcoins.
  This number can be encoded into e.g. a QR code or a text string for easier
  communication. 
* Satoshi: a bitcoin microcent: 1/100000000 bitcoin. 

# What is 'a Bitcoin'
To understand the answer, you must understand that there is no clear answer
to the question "What is 'a dollar'?". Is it a single 1 dollar bill, four 
quarters or 'the current amount in my bank account'? Is it 'the amount of money 
sufficient to buy a cheap bread at Walmart in 2014'?

A better question is: what is 'ownership of an amount of bitcoin'? You own
an amount of bitcoin if someone has transferred that amount to an address you
own. Ownership of an address is proven by the fact that you own the secret 
private key that belongs with the public key that the address was generated from. 
Only you, with that private key, can create a transaction that transfers funds 
from that address to another address.

# The bootstrapping problem
By now there are several other types of 'electronic cash' like Bitcoin, called
['cryptocurrencies'](https://en.wikipedia.org/wiki/Cryptocurrency). The questions
each of these currencies initially face are: 

* why would anyone accept a few bytes of data as payment? Why do these bytes 
  have value and can they be traded for tangible goods?
* How does anyone get the first coins of a new cryptocurrency? How does the
  first block get into the ledger?

The answer to the first is: why would anyone accept gold? You can't eat it,
you can't easily convert it into more liquid currency and its intrinsic,
industrial, value is much lower than its market price. Yet people are willing
to pay good money for gold. In other words, the value of goods is
[subjective](https://en.wikipedia.org/wiki/Subjective_theory_of_value). Accepting
bitcoins is a matter of trust: the trust that you will be able to spend them 
later for something of equal value to the good you originally received the
bitcoins for.

The answer to the second is: the Bitcoin protocol provides a way to start the
blockchain with a 'Genesis block'. The creator of a cryptocurrency can ensure
he receives the reward for that block and start spending it to anyone who
will accept it (see previous paragraph), making new transactions, which require
new blocks to be appended to the chain, etc. There are some challenges here
(the author has plenty of time to be the only one mining blocks, receiving a
lot of the new coins himself, in the beginning it is easy for a malicious user
to take over the network by providing more than 51% of the mining capacity),
but all in all that is only relevant if you want to start a new cryptocurrency.

# So, what is the blockchain?
A series of transactions encoded in a data structure shared between all members
of a community. These transactions have an agreed upon order, cannot be revoked 
or denied, and are, for most practical purposes, instantaneous.

The really interesting things start happening when you consider two things
in addition to everything above:

* the ability to embed messages in transactions
* the ability to provide scripting abilities that make transactions subject
  to certain conditions that must have been fulfilled before they are valid

# And what can you use it for?
Interesting ideas are:
* publicly proving ownership of a certain document, by embedding the hash of
  the document as a message in a transaction
* lotteries between many parties that are guaranteed to be fair and do not need
  oversight by a central/3rd party that requires fees for its service


Sources:

* [Plenty of Google queries](https://google.com)
* [Wikipedia](https://wikipedia.com)
* [https://en.bitcoin.it/wiki/Myths](https://en.bitcoin.it/wiki/Myths)
* [https://bitcoin.org/en/faq](https://bitcoin.org/en/faq)

