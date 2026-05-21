+++
date = '2026-05-18T20:42:01+02:00'
title = 'My Companies And Projects'
categories = ["Career"]
image = './featured.jpg'
+++

As a contractor, I've worked for many companies (contracts are typically short) and taken part in numerous projects. The following is a short summary of those companies and the most important and interesting projects I participated in.

<!--more-->

## Kraken ([kraken.com](https://kraken.com))

{{< figure src="./kraken.png" alt="Kraken logo" class="block me-auto ms-0" >}}

A world-leading cryptocurrency exchange and Web3 pioneer known for its uncompromising commitment to security, engineering excellence, and financial freedom. As a developer-centric organization, Kraken fosters a high-performance culture that tackles complex distributed systems challenges, high-throughput trading engine optimization, and cutting-edge blockchain integrations to provide a seamless, secure experience for millions of global users.

### Integration of Equities

Integration of the trading platform with third-party equity service providers, allowing Kraken users to trade regular stocks from the main platform and even pay for them using cryptocurrency.

### Re-architecture and Implementation of a Horizontally Scaling Trading Engine

A massive architectural redesign with one main goal in mind: making the core trading engine horizontally scalable while maintaining ultra-low latency requirements. A major part of the work involved providing alternative protocols and algorithms to maintain Kraken's existing offerings while changing the internal implementation to allow it to scale. My initial estimates showed around 10x improvements, and an early prototype successfully validated the core flow.

### Scope-Driven Configuration Service

A global service needs to maintain distinct sets of configurations for different countries, regions, client attributes, and even individual VIP clients. In a regulated market, every change must be approved by more than one person, and some values need to be dynamically calculated from live market data. Configuration changes must be delivered to all applications within milliseconds, maintaining strict transactional and ordering consistency.

## Ardigen ([ardigen.com](https://ardigen.com))

{{< figure src="./ardigen_logo.jpg" alt="Ardigen logo" class="block me-auto ms-0" >}}

A leading generative AI company at the intersection of biology and data science, dedicated to accelerating precision medicine. Ardigen provides a sophisticated environment for software engineers to build advanced platforms that leverage machine learning and bioinformatics to decode the complexity of the human immune system, directly contributing to the development of life-saving cancer therapies and personalized treatments.

### Biological Pipelines

Researching cancer using genetic information requires processing enormous amounts of data. Typical calculations would require months or even years of processing on a single machine. To shorten this timeline, hardware virtualization systems (such as AWS Batch) are paired with tools that automate jobs on those virtual machines. The most popular tool at the moment is Nextflow, which allows you to define pipelines as directed acyclic graphs (DAGs) of jobs that can be run across a variety of virtualization technologies. The purpose of my project was to develop several medical pipelines designed to help scientists test immunological vaccines, microbiological treatments, and classic pharmacological drugs.

## GlobalLogic ([globallogic.com](https://www.globallogic.com))

{{< figure src="./gl_logo.png" alt="Global Logic logo" class="block me-auto ms-0" >}}

A Hitachi Group Company and leader in digital product engineering that helps brands design and build innovative software for the modern economy. For programmers, GlobalLogic offers a dynamic, multi-industry environment where engineering teams solve large-scale challenges in cloud platforms, embedded systems, and experience design, enabling the digital transformation of world-class businesses through high-quality code and agile methodologies.

### Automotive Message Router

In the old days, mechanics checked cars using their eyes, hands, and sometimes even their noses. These days, however, cars are essentially computers with an accidental ability to drive. Most diagnostics therefore happen via electronic interfaces using self-reflective software. In order to test numerous internal chips, a high volume of messages is sent over an internal network to check if a given device responds, collect metrics, and diagnose any detected deviations. Mechanics simply connect to the car and download all the necessary data.

## Collective Sense ([collective-sense.com](https://collective-sense.com))

{{< figure src="./cls-logo-original.png" alt="Collective Sense logo" class="block me-auto ms-0" >}}

An innovative, US-based cybersecurity startup (later acquired by Sumo Logic) that specialized in building advanced, machine learning-driven network security and threat-hunting platforms. For a programmer, Collective Sense offered a fast-paced R&D environment focused on big data analytics, custom SIEM/IDS (Security Information and Event Management / Intrusion Detection System) software engineering, and high-performance backend systems designed to detect and neutralize complex cyber threats in real time.

### Network Topology Discovery

A network discovery tool that performed active network scans to search for hosts and network devices, and was capable of iterating over connected routers to map the entire network. This was a heavy R&D project because there are virtually no standard methods for discovering network topology, and networks were never designed with providing metadata in mind. The application was partially based on algorithms found in OpenNMS (the only major open-source application with the same goal), but that tool offered only basic functionality. New algorithms and methods had to be created from scratch based on the limited information provided by the specific device models used by our clients.

The basic approach behind network discovery is to obtain information from devices using the SNMP protocol, command-line consoles, open ports, or by parsing their management websites (as with some Cisco devices). This yields small pieces of information about the network, which you must then combine into a big picture using custom algorithms. Sometimes, clever hacks and guessing have to replace solid information. The algorithms also need to correctly handle cases where there isn't enough information available, leaving the system to choose the most likely connection patterns.

Unfortunately, there are thousands of different device models, hundreds of existing network protocols, hundreds of standards describing how devices should exchange data, and each device model has thousands of bugs. This made it impossible to create an application that would work flawlessly in *any* existing network, so we settled for a simpler solution capable of correctly discovering around 95% of typical network schemas.

## Delphi ([delphi.com](http://delphi.com))

{{< figure src="./Delphi-Logo-Jan-2015.png" alt="Delphi logo" class="block me-auto ms-0" >}}

Aptiv (formerly Delphi Automotive) is a global technology leader and automotive pioneer that renamed itself Aptiv following a strategic evolution to focus purely on the future of mobility. For software engineers, Aptiv provides a world-class platform to develop advanced safety systems, autonomous driving technologies, and vehicle-to-everything (V2X) connectivity. Programmers here work at the absolute cutting edge of embedded systems, real-time sensor fusion, and high-performance computing to redefine how the world moves.

### HTML Remote Car Radio Manager

During development, car radios do not have a built-in user interface. To test them during test drives, a temporary interface is required. The purpose of this project was to create an HTML interface that connects to the radio via WebSockets to send control messages. The interface also displayed information about available stations, signal quality, and real-time debug data.

## i2a Solutions ([i2asolutions.com](http://i2asolutions.com))

{{< figure src="./logo-1.png" alt="i2a Solutions logo" class="block me-auto ms-0" >}}

An agile software development studio and digital consulting agency focused on building high-performance web and mobile applications. For a programmer, i2a Solutions provides an interactive, full-stack environment centered on clean architecture, responsive user experiences, and tailored digital solutions—ranging from ad delivery engines to augmented reality (AR) and virtual reality (VR) integrations—enabling engineers to turn creative concepts into scalable, market-ready software products.

### Scalable Real-Time Bidding Platform ([moasis.com](https://www.moasis.com/))

This was my biggest commercial success—an application that generated around $20,000,000 USD over several years. Its main purpose was to run real-time advertising auctions, allowing clients to display ads on mobile phones and websites. When I started working for Moasis (an i2a Solutions client), their solution could only handle around 200 auctions per second. I became the sole developer and, consequently, the architect of the entire underlying system. There were two main challenges: the company wanted to drastically increase processing capabilities, and response times were incredibly critical—anything above 50 milliseconds would make the system unusable due to third-party requirements.

The new version was fully scalable and easily handled 80,000 requests per second, with the ability to scale further to cover all available ad traffic. The average response time was around 1 ms, with a maximum response time of about 5 ms. The core challenges revolved around scaling, ultra-low latency, and implementing algorithms that distributed ad spending smoothly across different geographical locations and time zones.

## DreamLab/Onet ([onet.pl](http://onet.pl))

{{< figure src="./2000px-Onet.pl_logo.svg_.png" alt="Onet logo" class="block me-auto ms-0" >}}

Poland's flagship online portal and largest digital news platform, reaching millions of active users daily. For web developers and software engineers, Onet offers a high-stakes, massive-scale environment centered on high-availability system architecture, real-time content delivery networks (CDNs), and complex ad-tech integrations. Programmers here work on highly optimized backend frameworks and performance-critical frontend systems designed to process heavy traffic loads smoothly while driving digital media innovation.

### Content Management Platform

Onet maintains hundreds of thousands of articles, millions of photos, and a massive library of videos, with new content added daily. Additionally, these articles serve as a vehicle for advertisements, meaning they must integrate with tools that measure and boost ad effectiveness. The main content management system at Onet is essentially WordPress on steroids, featuring a custom format and hundreds of features tailored specifically for company workflows and task automation.

Part of my job also included migrating legacy articles into the new format, combined with a massive data cleanup.

## IBM ([ibm.com](https://ibm.com))

{{< figure src="./IBM.png" alt="IBM logo" class="block me-auto ms-0" >}}

A global technology titan and R&D powerhouse that has continuously redefined the computing industry for over a century. For software engineers, IBM offers an elite environment to work on foundational technologies, pushing the boundaries of hybrid cloud architecture, enterprise open-source systems, and trailblazing developments in enterprise artificial intelligence (watsonx) and quantum computing. Programmers here design robust, secure, and massive-scale infrastructure that powers the world's most critical financial, healthcare, and enterprise networks.

### Scalable Agent Network

A fully scalable agent network capable of handling several million connected devices while providing bidirectional message exchange between subservices and subagents.

### Remote Computer Scanner

A subagent utilizing the previously mentioned scalable agent network. Its purpose was to perform scans on connected devices and inventory installed software.

### Netezza Database Management Component

Netezza was acquired by IBM due to its phenomenally fast databases built for business analytics. The system takes the form of a massive hardware rack packed with dozens of hard drives, CPUs, FPGAs, network components, and power units. This project involved maintaining and extending a management component—for example, implementing features to extract detailed hard drive health data via the SMART protocol, and preventing false positives by delaying alerts using several configurable metrics.

## Qnective ([qnective.com](http://qnective.com))

{{< figure src="./100016882-logo-qnective-ag.png" alt="Qnective logo" class="block me-auto ms-0" >}}

A Swiss-based cybersecurity and secure telecommunications company specializing in high-grade encrypted communication platforms for governments, defense sectors, and public safety organizations. For a programmer, Qnective offers an elite, R&D-driven workspace centered on advanced cryptography, secure VoIP architectures, and multi-layer network infrastructure. Developers here solve sophisticated challenges in data confidentiality and wireless networking to build tap-proof mobile frameworks that protect critical global communications.

### Symbian VoIP Client

A seamless Symbian VoIP client that integrated with the phone's native calling features and streamed audio over an IP stack using proprietary protocols. The main goal of the project was to define the communication protocol between servers and clients, and to implement a SIP server running on `localhost` directly on the Symbian device (which was the only way to seamlessly force all calls through the VoIP channel instead of the standard mobile network).

### BlackBerry VoIP Client

Maintenance and improvement of a BlackBerry VoIP client. This project primarily involved R&D regarding playing real-time audio from an incoming RTP stream—something BlackBerry devices did not support out of the box.

## WindMobile ([windmobile.pl](http://windmobile.pl))

{{< figure src="./logo_color_pantone_wm.png" alt="Wind Mobile logo" class="block me-auto ms-0" >}}

Ailleron (formerly Wind Mobile) is an innovative, Kraków-based technology pioneer that rebranded as Ailleron following its successful expansion and merger with Software Mind. Originally starting as a high-growth startup, the company became famous as the premier creator of Value-Added Services (VAS) for major telecom operators, building massive-scale mobile platforms like *"Granie na Czekanie"* (T-Mobile) and *"Czasoumilacz"* (Plus). For software engineers, it provided a dynamic, high-load R&D environment to develop heavy-traffic audio-streaming systems, mobile marketing engines, and complex cellular network integrations.

### NaviVoice

This application handled phone signaling and media using specialized network cards from Dialogic. Its main purpose was to provide a much simpler, protocol-independent API for business-layer applications. It supported several analog protocols, SS7, ISDN, SIP, and various less common telecom protocols. Its typical use case was running ringback tone playbacks (the music played to a caller before you answer). The project involved maintenance, bug fixing, migrating from Windows to a cross-platform solution (primarily Windows and Linux), and implementing the BICC protocol (part of the SS7 protocol stack).

### Video Streaming Platform for Mobile Phones

An R&D project focused on streaming MPEG-4 video over 3G networks (video calls). The application was capable of displaying multiple video streams and interactive menus, reacting in real time to user button presses on their mobile phones. The project utilized hardware network cards manufactured by Dialogic, working directly with their drivers and APIs.