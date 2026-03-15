---
title: "Networks"
date: 2021-05-24T20:32:11+05:30
tags:
  - network
---

<p>Here is the stuff I read related to networks, different models and their working,</p>
<h2 id="osi-model">OSI Model<a href="#osi-model" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>The OSI Model (Open Systems Interconnection Model) is a conceptual framework used to describe the functions of a networking system. It defines rules and requirements that support effortless data transfer between different products and software. In the OSI reference model, the communications between a computing system are split into seven different abstraction layers: Physical, Data Link, Network, Transport, Session, Presentation, and Application.</p>
<figure>
    <img src="https://i.imgur.com/zau6tPo.png"/> 
</figure>

<h3 id="application">Application<a href="#application" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>The Application layer is the OSI layer closest to the end-user. In this layer, the user connects to the network via software that uses the internet, like Chrome, Firefox, MS Suite.  It is important to note that these applications do not reside within the application layer. Instead, the application layer defines protocols (or rules) for the functioning of these applications.
Some examples of the protocols defined are:</p>
<ul>
<li>FTP: Used for file transfers</li>
<li>HTTP/S: Used for web surfing</li>
<li>SMTP: Used for emails</li>
<li>Telnet: Used for virtual terminal.</li>
</ul>
<p>When data is given to the Application layer, it is passed down into the Presentation layer.</p>
<h3 id="presentation">Presentation<a href="#presentation" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>The Presentation layer receives data from the Application layer and converts it into machine language. It has three main tasks:</p>
<ul>
<li>Conversion: Converts the machine-specific data from the Application layer into a standardised format that the application layer could understand in the receiving computer.</li>
<li>Compression: Often, the binary code after conversion is too large. It is then compressed to reduce the memory it takes.</li>
<li>Encryption: As an additional layer of security, the compressed data is then encrypted using SSL (Secure Sockets Layer).</li>
</ul>
<p>After these three tasks are done, the data is then transmitted to the Session layer. \</p>
<h3 id="session">Session<a href="#session" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>The Session layer controls the conversations between different computers. On receiving the data, it looks to connect with the other computer across the network. If it can not be done, then it returns an error message. If a session can be established, the Session layer is in charge of maintaining it. The Session layer also needs to co-operate with the Session layer of the other computer to synchronise communications. Once communication is over, the Session layer terminates the connection as well.</p>
<p>The Session layer creates unique sessions per communication request, due to which you can make multiple requests to different endpoints simultaneously (multiple tabs on a browser).</p>
<p>Once we get the data from the network, it is converted into data packets and sent to the Transport layer.</p>
<h3 id="transport">Transport<a href="#transport" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>The Transport layer receives data packets from the Session layer and chooses the protocol over which the data is to be transmitted.</p>
<ul>
<li>TCP: Transmission Control Protocol. It is connection-based transmission, which means that connection between the computers is established and maintained throughout the request. These layers are slower since it has a feedback mechanism. Via the feedback, it is ensured that the lost/corrupted data can be retransmitted. It is generally used for EMails, Loading Webpage, File Transfer.</li>
<li>UDP: User Datagram Protocol. It is connectionless transmission, where there is a high chance of data being lost. It is up to the receiving computer to keep up with the data which is sent. In this case, data transportation is faster, but due to the lack of feedback mechanism, loss/corrupted data cannot be regained. It is generally used for Video Streaming, Calls.</li>
</ul>
<p>The Transport layer divides the data into bite-sized pieces, where each piece contains a Source and Destination Port Number and Sequence Number.</p>
<ul>
<li>Port Number: Used to direct each segment to the correct Application</li>
<li>Sequence Number: Used to reform the whole message in the correct order.</li>
</ul>
<p>These bite-sized pieces are called Segments if using TCP and datagram if using UDP.  They are then sent to the Network layer.</p>
<p>The Transport layer is also in charge of flow control. If the server sends data at a higher rate than our machine can handle, it communicates and asks to send data at a lower rate. If the server sends data at a lower rate, it communicates to send data faster. In TCP, if data is corrupted, it uses Automated Repeat Request to restore the data.</p>
<h3 id="network">Network<a href="#network" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>The Network layer is responsible for locating the destination of your request. The network layer takes the IP address (if you want to request information from a webpage) for the page and imgs out the best route to take.</p>
<p>Logical Addresses (here, IP Address) are used to provide order to networks, categorising them. Each segment is assigned a sender and receiver IP address, which is called a packet. These packets travel through the network.</p>
<p>The process of moving data packets from source to destination is called routing.</p>
<h3 id="data-link">Data-Link<a href="#data-link" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>Data packets are sent from the Network layer to the Data-Link layer. The Data-Link layer adds a MAC (Media Access Control) Address to each packet for sender and receiver to make a Frame. Inside every network-enabled computer is a NIC (Network Interface Card) that comes with a unique MAC.</p>
<p>The Data-Link layer serves another essential function. It checks if the received information is corrupted during transmission or not.</p>
<h3 id="physical">Physical<a href="#physical" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h3>
<p>The lowest layer of the OSI Model. The Physical layer receives data from the Data-Link layer as frames and converts them into binary format. Once it is converted into binary, it is then transmitted as electrical pulses.</p>
<h2 id="receiving">Receiving<a href="#receiving" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<p>When the second computer receives the message, it reverses the process. It starts at the physical layer and sends the data up to the application layer, stripping the added information (de-encapsulation) as it goes up.</p>
<figure>
    <img src="https://i.imgur.com/b0D2wWQ.png"/> 
</figure>

<h2 id="references">References<a href="#references" class="anchor" aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 7h3a5 5 0 0 1 5 5 5 5 0 0 1-5 5h-3m-6 0H6a5 5 0 0 1-5-5 5 5 0 0 1 5-5h3"></path><line x1="8" y1="12" x2="16" y2="12"></line></svg></a></h2>
<ul>
<li>TryHackMe Introductory Networking</li>
<li>OSI Model Explained by TechTerms</li>
</ul>
