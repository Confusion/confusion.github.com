---
layout: post
title: ! 'Writers and editors: don''t make me feel stupid!'
tags:
- wikipedia
- writing
- web services
---
Imagine you don’t know what a [web_service](http://en.wikipedia.org/wiki/Web_service) is, but you’ve heard someone talking
about them and you are interested in knowing more about them. You click the
link in the previous sentence and are told
     Web services today are frequently just Application Programming
     Interfaces (API) or web APIs that can be accessed over a network,
     such as the Internet, and executed on a remote system hosting the
     requested services.
That didn’t make any sense to you, but you continued reading and made it to
     The W3C defines a “web service” as “a software system designed to
     support interoperable machine-to-machine interaction over a network.
which started to make a bit of sense. However, as you weren’t particularly
enlightened yet, you enter ‘What_is_a_webservice’ into Google and read other
['explanations'](http://www.google.com/search?ie=UTF-8&q=what+is+a+web+service) like the one from [Webopedia](http://www.webopedia.com/TERM/W/Web_Services.html):
     The term Web services describes a standardized way of integrating
     Web-based applications using the XML, SOAP, WSDL and UDDI open
     standards over an Internet protocol backbone. XML is used to tag the
     data, SOAP is used to transfer the data, WSDL is used for describing
     the services available and UDDI is used for listing what services are
     available. Used primarily as a means for businesses to communicate
     with each other and with clients, Web services allow organizations to
     communicate data without intimate knowledge of each other’s IT
     systems behind the firewall.
There’s not a doubt on my mind that most people are now left feeling stupid and
confused. This is a very bad thing, because it discourages people from
learning: if it is impossible to find a decent introduction to a subject,
surely it’s either too hard or too obscure?
There are several possible explanations for why these introductions are so bad:

* Perhaps it is simply too hard to introduce a subject using common English
  terms?
* Perhaps the authors have an advanced understanding of the subject, but
  not of writing for a general audience?
* Perhaps they suffer from the disease of overuse of jargon to which people
  in every profession fall prey?
* Perhaps they feel the quality of an article suffers when a web service
  would be introduced in terms a layman could follow, like
      A web service is a service offered by a piece of software that allows
      one computer to receive and answer requests for information from
      other computers over a network

I’m not sure which of these reasons is dominant, but even if all of them hold
for the authors: surely there is at least one editor that realizes it should be
thumbed down and can explain how and why?

What motivated me to write this article was the fact that, for the first time,
I had to use WS-Addressing and WS-Security in a web service client and the
first question that came to mind was: “why do we need these”? After figuring
that out, I concluded that the explanations on Wikipedia and other high-ranking
search results really didn’t give me the answers I was looking for. 

Firstly they suffered from the problems described in the beginning of this post, but
secondly: the explanations never went into the ‘why’ of these things. They all
just explained the ‘what’ and the ‘how’. This is what the Wikipedia
introduction on WS-Addressing currently says:
     WS-Addressing or Web Services Addressing is a specification of
     transport-neutral mechanisms that allow web services to communicate
     addressing information. It essentially consists of two parts: a
     structure for communicating a reference to a Web service endpoint,
     and a set of Message Addressing Properties which associate addressing
     information with a particular message.
Now I don’t like feeling stupid and I don’t like having to think longer than is
strictly necessary to understand something and this causes both. Why wouldn’t
something like the following do?
     The motivation for WS-Addressing is that you want to allow clients to
     invoke a web service using address A, while the web service itself
     lives at address B. This allows you to, for instance, use address A
     as a front door to web services at addresses B, C, D and E, while
     having the software at address A perform routing, validation,
     authentication, etc. for all these other web services. It also allows
     you more freedom in your web service addresses, because they don’t
     need to be URL’s that are reachable over the network: only address A
     needs to be ‘real’.
Secondly, a FAQ or the like should answer the obvious question: can’t you solve
this in some other way?
     Of course you can come up with your own scheme to obtain the same
     goal, but one downside to that is that the software at address A
     would have to be able to deal with different homegrown
     implementations of the same general principle, if it were to allow
     access to web services of different origins. Another downside is that
     you may initially overlook functionality, that proves harder to ‘tack
     on’ later. That’s why Microsoft, IBM, Sun, BEA and SAP, under the
     auspices of the W3C, came up with a single standard, which contains
     specifications for all scenarios they could think of.
