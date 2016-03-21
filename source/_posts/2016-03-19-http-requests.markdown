---
layout: post
title: "Because the Internet Part 1: History and TCP/IP"
date: 2016-03-19 16:11:20 -0700
comments: true
categories: http internet ip tcp protocol web networks
---
###Introduction
So on a semantic level, everyone knows what the internet is, and it's definitely a thing that has dramatically changed the world. The speed of information transfer is doing incredible things, both by making it faster and cheaper to get information, which has democratizing effects, and also by enabling people to stay in touch, it's incredible really. And in the following series of blog posts, I am going to try and cover the main big ideas about what makes the internet what it is, and how it works.

###First, a little history
So it doesn't seem like Al Gore actually invented the internet. It seems to have started as early as the 1950's, when the idea of networking and [packet switching](https://en.wikipedia.org/wiki/Packet_switching), which is the method by which computers send chunks of data to each other and can talk to many of each other at the same time, was the method these networks were using. Development continued for several decades, and in 1981, the National Science Foundation funded the Computer Science Network, and the Internet protocol suite (TCP/IP) became the standard networking protocol.
In the late 1980's, commercial [internet service providers](https://en.wikipedia.org/wiki/Internet_service_provider) started coming onto the scene. TCP/IP, which is also known as the Internet Protocol suite, became the standard method of exchanging information on the internet in March 1982, when the Department of Defense declared it so, and became open source in 1989. Officially commercial entities started using the internet around 1990, and by 1995 the world wide web was pretty much in full force. So this stuff has actually been around for a long time, but it's really exploded in the last 20 years or so.

###And now, TCP/IP!!
So what the hell is TCP/IP?
TCP stands for Transmission Control Protocol, and IP stands for Internet Protocol. It is the basic communication language for the world wide web, as well as private networks (both extranet and intranet networks). Obviously, there are 2 main parts - 1. TCP and 2. the IP.

1. *TCP* (Transmission Control Protocol) - This is the higher layer. TCP manages the chunking of data of whatever is trying to be sent, breaking the file or files into small packets of data, and then reassembling the chunks once they've been transmitted.
2. *IP* (Internet Protocol) - This is the lower layer. The IP handles the addresses for each packet, and makes sure that they get to the right place.

One term that is often mentioned in the same sentence is the concept of being "stateless." Broadly, this just means that the network doesn't remember anything about the last time it sent packets; it's like it has complete and utter lack of memory. There are no connections between packets; just discrete and separate requests. This really frees things up, so that multiple computers can use servers at once and no one has a lock or can delay these transactions.

###Ok, seems good so far. Now let's get into the guts of the thing.
The easiest way to understand the structure of the protocol is by looking at its anatomy. There are 4 different layers of abstraction, starting with closer to the user, and then getting more nitty gritty into the actual transmission. Abstraction is great because it frees things up for the top user; they don't need to focus on the implementation of the very lowest level stuff. Similarly, the lowest level doesn't need to care about what it's being sent, it just kinda blindly stupidly does what it's supposed to do. It's beautiful. The 4 layers are:
1) The Application Layer - where you create actual data and communicate it to people on different or the same host.
2) Transport Layer - the channel that you communicate on, and handling flow-control, connection establishment, and reliable transmission of data.
3) Internet Layer - defines the addressing and routing structures used, Its function in routing is to transport datagrams to the next IP router that has the connectivity to a network closer to the final data destination
4) Link Layer - defines the networking methods within the scope of the local network link on which hosts communicate without intervening routers. 

Let's take a look at each in turn, starting low level.
###The Link Layer
The Link Layer (also known as the Data-Link layer) deals with local networks. ensures that an initial connection has been set up, divides output data into data frames, and handles the acknowledgements from a receiver that the data arrived successfully. It also ensures that incoming data has been received successfully by analyzing bit patterns at special places in the frames.

###Internet Layer
The Internet layer handles routing between networks. It's responsible for transferring data between the source and destination computers. The Internet layer accepts data from the Transport layer and passes the data to the Network Interface layer. One function is routing the data to the correct destination. This layer takes care of sending the data through the shortest route if more than one route is available. In addition, if a route through which a datagram is to be sent has problems, the datagram is sent through an alternate route.

###Transport Layer
The transport layer, in contrast, is sufficiently conceptual that it no longer concerns itself with these “nuts and bolts” matters. It relies on the lower layers to handle the process of moving data between devices.The transport layer is charged with providing a means by which these applications can all send and receive data using the same lower-layer protocol implementation. Thus, the transport layer is sometimes said to be responsible for end-to-end or host-to-host transport.

###The Application Layer
At the very top of the OSI Reference Model stack of layers, we find layer 7, the application layer. Continuing the trend that we saw in layers 5 and 6, this one too is named very appropriately: the application layer is the one that is used by network applications. These programs are what actually implement the functions performed by users to accomplish various tasks over the network. Most people, when connected to the internet, are using higher level protocols, like the HTTP protocol, but it's all built on and based off of TCP/IP and this is the layer HTTP is in.

###Conclusion
Well there you have it, that's part one. It's a fascinating and important thing for networking, and while we sit much higher in abstraction now, we all still depend on it. There are trillions of data packets exchanged daily, and it's all built on this model. Next time, I'll delve a little more into both TCP and IP individually.
