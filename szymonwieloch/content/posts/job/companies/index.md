+++
date = '2026-05-18T20:42:01+02:00'
title = 'My Companies And Projects'
categories = ["Job"]
image = './featured.jpg'
+++

As a contractor, I've worked for many companies (contracts are typically short) and taken part in many projects. The following is a short summary of all those companies and the most important and interesting projects I participated in.

<!--more-->

## Kraken ([kraken.com](https://kraken.com))

{{< figure src="./kraken.png" alt="Kraken logo" class="block me-auto ms-0" >}}

A world-leading cryptocurrency exchange and Web3 pioneer known for its uncompromising commitment to security, engineering excellence, and financial freedom. As a developer-centric organization, Kraken fosters a high-performance culture that tackles complex distributed systems challenges, high-throughput trading engine optimization, and cutting-edge blockchain integrations to provide a seamless, secure experience for millions of global users.

### Integration of Equities

Integration of the trading platform with a 3rd party equity service providers that allowed users of Kraken trading regular stocks from the main platform, and even pay for them in crypto.

### Re-architecture and implementation of a Horizontally Scaling Trading Engine

Massive architectural redesign with one main goal in mind: make the core trading engine horizontally scalable while maintainig the ultra low latency requirements. Big part of the work was providing alternative protocols and algorithms to maintain the current Kraken offering while changing the internal implementation to make it scale. My initial estimates showed around 10x improvements and an early prototype validated core flow.

### Scope-driven configuration service

A global service needs to maintan a set of separate configuration for different countries, regions, client atrributes and even individual VIP clients. In a regulated market every change needs to be approved by more than one person. Some values need to be dynamically calculated from the market data. Configuration on change needs to be delivered to all application in milliseconds and certain transactional and ordering consistency needs to be maintained.

## Ardigen ([ardigen.com](https://ardigen.com))

{{< figure src="./ardigen_logo.jpg" alt="Ardigen logo" class="block me-auto ms-0" >}}

A leading generative AI company at the intersection of biology and data science, dedicated to accelerating precision medicine. Ardigen provides a sophisticated environment for software engineers to build advanced platforms that leverage machine learning and bioinformatics to decode the complexity of the human immune system, directly contributing to the development of life-saving cancer therapies and personalized treatments.

### Biological pipelines

Research on cancer using genetic information requires enormous amounts of data to be processed. Typical calculations require monts or years of processing on a single machine. To shorten this process hardware virtualization systems were created (i. e. AWS Batch) together with some tools that can automate jobs on those virtual machines (i. e. NextFlow). The most popular tool is at the moment NextFlow that allows you to define pipelines – directed acyclic graphs of jobs that can be run on a bunch of different virtualization technologies. The purpose of my project was development of several medical pipelines designed to help scientists to test immunological vaccines, microbiological treatments and classic pharmacological drugs.

## Global Logic ([globallogic.com](https://www.globallogic.com))

{{< figure src="./gl_logo.png" alt="Global Logic logo" class="block me-auto ms-0" >}}

A Hitachi Group Company and leader in digital product engineering that helps brands design and build innovative software for the modern economy. For programmers, GlobalLogic offers a dynamic, multi-industry environment where engineering teams solve large-scale challenges in cloud platforms, embedded systems, and experience design, enabling the digital transformation of world-class businesses through high-quality code and agile methodologies.

### Automotive message router

In the old days mechanics checked your car using eyes, hands and sometimes their noses. These days however cars are computers with an accidental ability of driving. Most of diagnostics therefore happens via electronical interfaces, using self-reflective software. In order to test numerous internal chips a bunch of messages gets sent over an ineternal network in order to check if the given device responds, collect metrics and diagnose the found deviations. Mechanics just connect to the car and download all the needed data.

## Collective Sense ([collective-sense.com](https://collective-sense.com))

{{< figure src="./cls-logo-original.png" alt="Collective Sense logo" class="block me-auto ms-0" >}}

An innovative, US-based cybersecurity startup (later acquired by Sumo Logic) that specialized in building advanced, machine learning-driven network security and threat-hunting platforms. For a programmer, Collective Sense offered a fast-paced R&D environment focused on big data analytics, custom SIEM/IDS (Security Information and Event Management / Intrusion Detection System) software engineering, and high-performance backend systems designed to detect and neutralize complex cyber threats in real time.

### Network topology discovery

Network discovery tool that performed active scans of network in search of hosts and network devices and that was capable of iterating over connected routers to map the whole network. This was a highly R&D project because there are basically no standard methods of discovering network topology and no network was designed with providing meta information in mind. The application was partially based on algorithms found in the sole open source OpenNMS application that had the same goal, but offered only basic algorithms. New algoithms and methods had to be created from scratch based on the available information provided by the specific models of devices used of our clients.

The basic approach behind network discovery is to obtain information from devices using SNMP protocol, console and open ports or by parsing their management website (some Cisco devices). This is how you get small pieces of information about the network. Then you need to combine these pieces into a big picture using your own algorithms. Sometimes hacks and guessing need to replace solid information. Your algorithms also need to correctly handle the cases when you don’t have enough information and the only what’s left is to choose the more likely connection patterns.

Unfortunately there are thousands of different device models, hundreds of existing network protocols, hundreds of standards that describe how the given devices should exchange information they posses and each device model has thousands of bugs. This made it impossible to create an application that would work in any existing network and settle for something much simpler that was capable of discovering correctly around 95% of network schemas – the most usual ones.


## Delphi ([delphi.com](http://delphi.com))

{{< figure src="./Delphi-Logo-Jan-2015.png" alt="Delphi logo" class="block me-auto ms-0" >}}

Aptiv (formerly Delphi Automotive) is a global technology leader and automotive pioneer that renamed itself Aptiv following a strategic evolution to focus purely on the future of mobility. For software engineers, Aptiv provides a world-class platform to develop advanced safety systems, autonomous driving technologies, and vehicle-to-everything (V2X) connectivity. Programmers here work at the absolute cutting edge of embedded systems, real-time sensor fusion, and high-performance computing to redefine how the world moves.

### HTML remote car radio manager

During development car radios do not have any human interface. To test them during test drives a temporal interface is required. The purpose of this project was to create a HTML interface that would connect to the radio using websockets an send control messages. The interface contained also information about available stations, quality and debug information.

## i2a Solutions ([i2asolutions.com](http://i2asolutions.com))

{{< figure src="./logo-1.png" alt="i2a Solutions logo" class="block me-auto ms-0" >}}

An agile software development studio and digital consulting agency focused on building high-performance web and mobile applications. For a programmer, i2a Solutions provides an interactive, full-stack environment centered on clean architecture, responsive user experiences, and tailored digital solutions—ranging from ad delivery engines to augmented reality (AR) and virtual reality (VR) integrations—enabling engineers to turn creative concepts into scalable, market-ready software products.

### Scalable real time bidding platform ([moasis.com](https://www.moasis.com/))

This was my biggest commercial success – an application that during several years earned around 20 000 000 USD. Its main purpose was to perform real time auctions of advertisement and allow clients to show their advertisement  on mobile phones and websites. When I started working for Moasis company(i2a Solutions client), their solution could only perform around 200 auctions per second. I became the only developer and, hence, an architect of the whole underlying system. There were two main issues there – the company wanted to highly increase the processing capabilities but also the time needed for a response was super crucial – anything above 50 milliseconds would simply made the system unusable because of third party requirements.

The new version was fully scalable and back then it was easily handling 80 000 requests per second, with the ability to scale to covert all available advertisement traffic. The average response time was around 1 ms, the maximum response time was around 5 ms. The core challenges were scaling, low latency and algorithms distributed spending over different geographical locations and different time zones.


## DreamLab/Onet ([onet.pl](http://onet.pl))

{{< figure src="./2000px-Onet.pl_logo.svg_.png" alt="Onet logo" class="block me-auto ms-0" >}}

Poland's flagship online portal and largest digital news platform, reaching millions of active users daily. For web developers and software engineers, Onet offers a high-stakes, massive-scale environment centered on high-availability system architecture, real-time content delivery networks (CDNs), and complex ad-tech integrations. Programmers here work on highly optimized backend frameworks and performance-critical frontend systems designed to process heavy traffic loads smoothly while driving digital media innovation.

### Content management platform

Onet maintains hundreds of thousands of articles, millions of photos and huge number of videos. New one are getting added every day. In addition to that, articles are used as a tool for showing advertisement and therefore need to be integrated with tools that measure and boost their effectiveness. The main content management system in One is a Wordpress on steroids, with a custom format, hundreds of features tailored specifically for the company and for automation of common tasks.

Also part of my job was conversion of the old articles in the new format, combined with a massive cleanup.

## IBM ([ibm.com](https://ibm.com))

{{< figure src="./IBM.png" alt="IBM logo" class="block me-auto ms-0" >}}

A global technology titan and R&D powerhouse that has continuously redefined the computing industry for over a century. For software engineers, IBM offers an elite environment to work on foundational technologies, pushing the boundaries of hybrid cloud architecture, enterprise open-source systems, and trailblazing developments in enterprise artificial intelligence (watsonx) and quantum computing. Programmers here design robust, secure, and massive-scale infrastructure that powers the world's most critical financial, healthcare, and enterprise networks.


### Scalable agent network

Fully scalable agent network that could handle several millions of connected devices and provide bidirectional message exchanges between subservices and subagents.

### Remote computer scanner

A subagent using the previously mentioned scalable agent network. Its purpose was to perform scans of connected devices and find installed software.

### Neteeza database management component

Neteeza is a corporation bought by IBM because of its phenomenally fast database created for business analytic purposes. It has a form of a huge rack with dozens of hard drives, CPUs, FPGAs, network components and power units. This project included maintenance and extension of a management component, for example implementation of features such as obtaining detailed information about hard drives using SMART protocol and preventing false alarms (false positives) by delaying alarms using several configurable metrics.

## Qnective ([qnective.com](http://qnective.com))

{{< figure src="./100016882-logo-qnective-ag.png" alt="Description" class="block me-auto ms-0" >}}

A Swiss-based cybersecurity and secure telecommunications company specializing in high-grade encrypted communication platforms for governments, defense sectors, and public safety organizations. For a programmer, Qnective offers an elite, R&D-driven workspace centered on advanced cryptography, secure VoIP architectures, and multi-layer network infrastructure. Developers here solve sophisticated challenges in data confidentiality and wireless networking to build tap-proof mobile frameworks that protect critical global communications.

### Symbian VoIP client

Symbian seamless VoIP client that integrated with a normal phone and then it streamed audio using proprietary protocols over IP stack. The main goal of the project was to define the protocol used between servers and clients and to implement SIP server that was running on localhost on Symbian phone (this was the only way to push all calls seamlessly via VoIP channel instead of the normal mobile network).

### Blackberry VoIP client

Maintenance and improvement of a Blackberry VoIP client. This project mainly included research on playing real time audio from incoming RTP stream (something that Blackberry did not support out-of-the-box).

## WindMobile ([windmobile.pl](http://windmobile.pl))


{{< figure src="./logo_color_pantone_wm.png" alt="Description" class="block me-auto ms-0" >}}


Ailleron (formerly Wind Mobile) - an innovative, Kraków-based technology pioneer that rebranded as Ailleron following its successful expansion and merger with Software Mind. Originally starting as a high-growth startup, the company became famous as the premier creator of Value-Added Services (VAS) for major telecom operators, building massive-scale mobile platforms like *"Granie na Czekanie"* (T-Mobile) and *"Czasoumilacz"* (Plus). For software engineers, it provided a dynamic, high-load R&D environment to develop heavy-traffic audio-streaming systems, mobile marketing engines, and complex cellular network integrations.

### NaviVoice

The application maintaned phone signaling and media using network cards from Dialogic company. Its main purpose was to provide much simpler and protocol independent API for business layer applications. It supported several analog protocols, SS7, ISDN, SIP and several less popular protocols. Its typical usage was running ring tone playbacks (a music that can be played instead of a normal ring tone when somebody is calling you). The project involved maintenance, fixing bugs, migration from Windows to multiplatform solution (mainly Windows and Linux) and implementation of BICC protocol that is a part of SS7 protocol stack.

### Video streaming platform for mobile phones

Research project using sending MPG4 videos over 3G network (video calls). The application was capable of displaying multiple videos and menus and reacting to users pressing buttons on mobile phones. The project used network cards manufactured by Dialogic company together with its drivers and API.