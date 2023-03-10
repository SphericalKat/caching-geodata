---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: '#1f222a'
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-left

fonts:
  sans: 'Plus Jakarta Sans'
  mono: JetBrains Mono
---

# Demystifying WebRTC

A gentle introduction to the protocols powering Audio/Video calls worldwide.


<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/SphericalKat/demystifying-webrtc" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout:default
---

# Why do I care?

- **Conferencing**
- **Broadcasting**
- **Remote Access**
- **File Sharing and Censorship Circumvention**
- **Internet of Things**
- **Media Protocol Bridging**

<!-- 
Conferencing - Conferencing is the original use case for WebRTC. The protocol contains a few necessary features that no other protocol offers in the browser. You could build a conferencing system with WebSockets and it may work in optimal conditions. If you want something that can be deployed in real world network conditions, WebRTC is the best choice.

WebRTC provides congestion control and adaptive bitrate for media. As the conditions of the network change, users will still get the best experience possible. Developers don’t have to write any additional code to measure these conditions either.

Broadcasting - WebRTC being in the browser makes it easy for users to publish video. It removes the requirement for users to download a new client. Any platform that has a web browser can publish video. Publishers can then send multiple tracks and modify or remove them at anytime. This is a huge improvement over legacy protocols that only allowed one audio or one video track per connection.

WebRTC gives developers greater control over the latency versus quality trade-offs. If it is more important that latency never exceeds a certain threshold, and you are willing to tolerate some decoding artifacts. You can configure the viewer to play media as soon as it arrives. With other protocols that run over TCP, that isn’t as easy. In the browser you can request data and that is it.

File Sharing and Censorship Circumvention - File Sharing and Censorship Circumvention are dramatically different problems. However, WebRTC solves the same problems for them both. It makes them both easily available and harder to block.

The first problem that WebRTC solves is getting the client. If you want to join a file sharing network, you need to download the client. Even if the network is distributed, you still need to get the client first. In a restricted network the download will often be blocked. Even if you can download it, the user may not be able to install and run the client. WebRTC is available in every web browser already making it readily available.

The second problem that WebRTC solves is your traffic being blocked. If you use a protocol that is just for file sharing or censorship circumvention it is much easier to block it. Since WebRTC is a general purpose protocol, blocking it would impact everyone. Blocking WebRTC might prevent other users of the network from joining conference calls.

Internet of Things - Internet of Things (IoT) covers a few different use cases. For many this means network connected security cameras. Using WebRTC you can stream the video to another WebRTC peer like your phone or a browser. Another use case is having devices connect and exchange sensor data. You can have two devices in your LAN exchange climate, noise or light readings.

WebRTC has a huge privacy advantage here over legacy video stream protocols. Since WebRTC supports P2P connectivity the camera can send the video directly to your browser. There is no reason for your video to be sent to a 3rd party server. Even when video is encrypted, an attacker can make assumptions from the metadata of the call.

Interoperability is another advantage for the IoT space. WebRTC is available in lots of different languages; C#, C++, C, Go, Java, Python, Rust and TypeScript. This means you can use the language that works best for you. You also don’t have to turn to proprietary protocols or formats to be able to connect your clients.

Media Protocol Bridging - You have existing hardware and software that is producing video, but you can’t upgrade it yet. Expecting users to download a proprietary client to watch videos is frustrating. The answer is to run a WebRTC bridge. The bridge translates between the two protocols so users can use the browser with your legacy setup.

Many of the formats that developers bridge with use the same protocols as WebRTC. SIP is commonly exposed via WebRTC and allows users to make phone calls from their browser. RTSP is used in lots of legacy security cameras. They both use the same underlying protocols (RTP and SDP) so it is computationally cheap to run. The bridge is just required to add or remove things that are WebRTC specific.

Data Protocol Bridging - A web browser is only able to speak a constrained set of protocols. You can use HTTP, WebSockets, WebRTC and QUIC. If you want to connect to anything else, you need to use a protocol bridge. A protocol bridge is a server that converts foreign traffic into something the browser can access. A popular example is using SSH from your browser to access a server. WebRTC’s data channels have two advantages over the competition.

WebRTC’s data channels allow unreliable and unordered delivery. In cases where low latency is critical this is needed. You don’t want new data to be blocked by old data, this is known as head-of-line blocking. Imagine you are playing a multiplayer First-person shooter. Do you really care where the player was two seconds ago? If that data didn’t arrive in time, it doesn’t make sense to keep trying to send it. Unreliable and unordered delivery allows you to use the data as soon as it arrives.

Data channels also provide feedback pressure. This tells you if you are sending data faster than your connection can support. You then have two choices when this happens. The data channel can either be configured to buffer and deliver the data late, or you can drop the data that hasn’t arrived in real-time.
 -->

---
transition: fade-out
layout: default
---

# What is WebRTC?

<br/>


- **Protocol** - Specifies how 2 agents can negotiate _bi-directional_ _secure_ _real-time_ communication.
- **API** - Allows developers to use the WebRTC protocol.
- **Open standard** - WebRTC is an open standard with multiple FOSS implementations.
- **Widely available** - All modern browsers support WebRTC.
- **Industry Standard** - The backbone of the Audio/Video call industry.
- **Mandatory encryption** - Communication between two peers is mandatorily encrypted.

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>

</style>

<!--
WebRTC is both a protocol and an API. The protocol specifies how 2 agents on the internet
can negotiate bidirectional, secure, real time communication.

The API allows you to use the protocol.

A similar relationship would be the one between HTTP and the Fetch API.
WebRTC the protocol would be HTTP, and WebRTC the API would be the Fetch API.

This list is not exhaustive, just an example of some of the things you may appreciate during your journey.
Don’t worry if you don’t know all these terms yet, we will go through them along the way.
-->

---
transition: slide-up
---

# Four steps to success

We can broadly divide the WebRTC protocol into four sections.

<br />


1. **Signaling** - How peers find each other in WebRTC.
2. **Connecting** - NAT Traversal with STUN/TURN
3. **Securing** - The transport layer with DTLS and SRTP
4. **Communicating** - With peers via RTP and SCTP


<!--
WebRTC solves a lot of problems. At first glance the technology may seem over-engineered, but the genius of WebRTC is its humility. It wasn’t created under the assumption that it could solve everything better. Instead, it embraced many existing single purpose technologies and brought them together into a streamlined, widely applicable bundle.

These steps are sequential, which means the prior step must be 100% successful for the subsequent step to begin.

One peculiar fact about WebRTC is that each step is actually made up of many other protocols! To make WebRTC, we stitch together many existing technologies. In that sense, you can think of WebRTC as being more a combination and configuration of well-understood tech dating back to the early 2000s than as a brand-new process in its own right.
 -->

---
layout: default
---

# Signaling

- **WebRTC agents** have no idea who to communicate with. Signalling is used to bootstrap the call.
- **Signaling** - uses an existing plaintext protocol called **SDP** (Session Description Protocol)
- Each **SDP** message is made up of Key-Value pairs, which contains information such as:
  - The **IPs** and **Ports** that the agent is reachable on (candidates).
  - The number of audio and video **tracks** the agent wishes to send.
  - The audio and video **codecs** each agent supports.
  - Some values used while connecting (`uFrag`/`uPwd`).
  - The **certificate fingerprint** used while securing the connection.
- WebRTC uses the **offer/answer** model. One WebRTC agent makes an "**Offer**" to connect, and the other agent "**Answers**" if it is willing to accept what has been offered.
- This gives the answerer a chance to **reject unsupported codecs**. This is how two peers can understand what formats they are willing to exchange.

<style>
    li {
        padding-top: 8px;
        @apply text-sm;
    }
</style>
<!-- 

This might seem a bit conterintuitive, but it makes sense if you think about it.

When a WebRTC Agent starts, it has no idea who it is going to communicate with or what they are going to communicate about. The Signaling step solves this issue. Signaling is used to bootstrap the call, allowing two independent WebRTC agents to start communicating.

Signaling uses an existing, plain-text protocol called SDP (Session Description Protocol). Each SDP message is made up of key/value pairs and contains a list of “media sections”. The SDP that the two WebRTC agents exchange contains details like:

(refer to slide content)

It is important to note that signaling typically happens “out-of-band”, which means applications generally don’t use WebRTC itself to exchange signaling messages.

many applications will simply use their existing infrastructure (e.g. REST endpoints, WebSocket connections, or authentication proxies) to facilitate trading of SDPs between the proper clients.

-->

---
layout: default
---

# SDP spec
Consists of Key-Value pairs. Following are the keys used by WebRTC.

- `v` - Version, should be equal to `0`.
- `o` - Origin, contains a unique ID useful for renegotiations.
- `s` - Session Name, should be equal to `-`.
- `t` - Timing, should be equal to `0 0`.
- `m` - Media Description `(m=<media> <port> <proto> <fmt> ...)`, described in detail later.
- `a` - Attribute, a free text field. This is the most common used line in WebRTC.
- `c` - Connection Data, should be equal to `IN IP4 0.0.0.0`.

---
layout: default
---

# SDP breakdown

<div grid="~ cols-2 gap-4">

<div>

```text {all|5|6|7-10|all}
v=0
o=- 3546004397921447048 1596742744 IN IP4 0.0.0.0
s=-
t=0 0
a=fingerprint:sha-256 0F:74:31:25:CB:A2:13:EC:28:6F:6D:2C:61:FF:5D:C2:BC:B9:DB:3D:98:14:8D:1A:BB:EA:33:0C:A4:60:A8:8E
a=group:BUNDLE 0 1
a=candidate:foundation 1 udp 2130706431 192.168.1.1 53165 typ host generation 0
a=candidate:foundation 2 udp 2130706431 192.168.1.1 53165 typ host generation 0
a=candidate:foundation 1 udp 1694498815 1.2.3.4 57336 typ srflx raddr 0.0.0.0 rport 57336 generation 0
a=candidate:foundation 2 udp 1694498815 1.2.3.4 57336 typ srflx raddr 0.0.0.0 rport 57336 generation 0
a=end-of-candidates

// media description
```

<!-- -->

</div>
<div>
 <ul>
 <li><code>fingerprint:sha-256</code> - This is a hash of the certificate a peer is using for DTLS. After the DTLS handshake is completed, you compare this to the actual certificate to confirm you are communicating with whom you expect.</li>

  <li><code>group:BUNDLE</code> - Bundling is an act of running multiple types of traffic over one connection. Some WebRTC implementations use a dedicated connection per media stream. Bundling should be preferred.</li>
  
  <li><code>candidate</code> - This is an ICE Candidate that comes from the ICE Agent. This is one possible address that the WebRTC Agent is available on. These are fully explained in the upcoming slides.</li>
 </ul>
</div>

</div>

<style>
    li {
        padding-top: 8px;
        @apply text-sm;
    }
</style>

---
layout: two-cols
---

# Media description in SDP

```text {all|4|5|6|1,9|10|11-14|all}
m=audio 9 UDP/TLS/RTP/SAVPF 111
c=IN IP4 0.0.0.0
a=setup:active
a=mid:0
a=ice-ufrag:CsxzEWmoKpJyscFj
a=ice-pwd:mktpbhgREmjEwUFSIJyPINPUhgDqJlSd
a=rtcp-mux
a=rtcp-rsize
a=rtpmap:111 opus/48000/2
a=fmtp:111 minptime=10;useinbandfec=1
a=ssrc:350842737 cname:yvKPspsHcYcwGFTw
a=ssrc:350842737 msid:yvKPspsHcYcwGFTw DfQnKjQQuwceLFdV
a=ssrc:350842737 mslabel:yvKPspsHcYcwGFTw
a=ssrc:350842737 label:DfQnKjQQuwceLFdV
a=msid:yvKPspsHcYcwGFTw DfQnKjQQuwceLFdV
a=sendrecv
```

::right::


- `mid` - Used for identifying media streams within a session description.
- `ice-ufrag` - This is the user fragment value for the ICE Agent. Used for the authentication of ICE Traffic.
- `ice-pwd` - This is the password for the ICE Agent. Used for the authentication of ICE Traffic.
- `rtpmap` - This value is used to map a specific codec to an RTP Payload Type. Payload types are not static, so for every call the offerer decides the payload types for each codec.
- `fmtp` - Defines additional values for one Payload Type. This is useful to communicate a specific video profile or encoder setting.
- `ssrc` - A Synchronization Source (SSRC) defines a single media stream track. `label` is the ID for this individual stream. `mslabel` is the ID for a container that can have multiple streams inside it.

<style>
    ul {
        padding-top: 4rem;
        padding-left: 16px;
    }
    li {
        @apply text-xs;
    }
</style>

<!-- 

-->

---
layout: default
---

# Step 2: Connection

 - Very often, the other WebRTC agent will not be on the same network. Calls will have to go over the public internet.
 - Some networks don’t allow UDP traffic at all, or maybe they don’t allow TCP. Some networks may have a very low MTU (Maximum Transmission Unit). There are lots of variables that network administrators can change that can make communication difficult.
 - Almost every user on the internet is behind a **NAT** (Network Address Translator). This helps ISPs avoid IPv4 exhaustion, but causes problems for WebRTC connections.
 - NAT, or Network Address Translation, is a method used by routers to allow multiple devices on a private network to share a **single public IP address**. NAT works by **modifying the IP addresses** of network packets as they pass through the router, **replacing the private IP** addresses of the devices on the network with the **public IP address of the router**. This allows the devices to access the internet without each needing its own public IP address.

<style>
    ul {
    }
    li {
        font-size: 1rem;
        line-height: 1rem;
        @apply pt-4;
    }
</style>

<!-- 
The problems with NAT being that only the router itself has a public IP, and individual devices on it don't;
so how does webrtc figure out which device to connect to? The answer is NAT mappings. In a nutshell, this is simply
a table maintained by the router where it maps a list of IP addresses that have contacted/been contacted by the local router
to a local port. 

This is very useful for having multiple devices share a single public IP address, but makes sending data to a 
particular device difficult. To get around this, we use STUN/TURN server.

-->

---
layout: default
---

# STUN
Session Traversal Utilities for NAT

- Existed before WebRTC
- Allows for the programmatic creation of NAT Mappings, and also gives you the mapping details that you can share with the other peer, so they can send traffic to you via the mapping you just created.
- Peers send a `STUN Binding Request` to the STUN server. The server responds with a `STUN Binding Response`, which contains an IP (the sender's public IP) and a port (the port bound by the NAT mapping).
- This information is sent to the other peer via the SDP exchange during signaling.
- However, the mapped address is not always helpful. Some NATs do not let in traffic unless the router has initiated connection with them first. In this case, packets are not allowed in through the mapping. To get over this, we use TURN.

---
layout: default
---

# STUN

<img src="/nat-stun.png" class="rounded-md">

---
layout: default
---

# TURN
Traversal Using Relays around NAT
- Used when direct connectivity isn't possible. Could be due to incompatible NAT types, or maybe the NATs don't speak the same protocol.
- Uses a dedicated server, which acts as a proxy for a peer. The client connects to a TURN Server and creates an `Allocation`. By creating an allocation, a client gets a temporary IP/Port/Protocol that can be used to send traffic back to the client. This new listener is known as the **Relayed Transport Address**.
- Disadvantages:
 - Increased latency/packet loss/jitter
 - TURN servers are expensive to host compared to STUN servers, as they require large amounts of bandwidth and resources.

<!-- 
Think of it as a forwarding address, you give this out so that others can send you traffic via TURN!

When you send outbound traffic via TURN it is sent via the Relayed Transport Address. When a remote peer gets traffic they see it coming from the TURN Server.
-->

---
layout: default
---

# ICE
Interactive Connectivity Establishment

- This is how WebRTC connects two agents by determining **all possible routes** between two peers and selecting the best.
- After connectivity is established, **any data** can be sent over it; it behaves like a regular socket.
- A route is defined as a **pair** of **local** and **remote** transport address.
- Each possible route is called a `Candidate Pair`. The ICE protocol finds the best route out of these.
- Both agents start sending traffic on each pair. Each pair that saw traffic gets promoted to a `Valid Candidate` pair.
The controlling agent nominates one of these pairs and attempt one more round of bi-directional communication. If this is successful, the nominated pair is used for the rest of the session.

<br>

For more information: https://katb.in/ice

<!--
These routes are known as candidate pairs, which are a pairing of a local and remote transport address.
This is where STUN and TURN come into play with ICE. These addresses can be your local IP address + port, 
a NAT mapping(STUN), or Relayed Transport Address (TURN). Each peer gathers all the addresses that they want to use,
exchange them over signaling using the SDP, and then attempts to connect.
-->

<style>
    li {
        font-size: 1rem;
        line-height: 1rem;
        @apply pt-4;
    }
</style>

---
layout: default
---

# Media Communication with WebRTC

 - Uses two pre-existing protocols: **RTP** and **RTCP**.
 - **RTP** (Real-time Transport Protocol) carries the media and was designed for real-time delivery.
 - **RTCP** (RTP Control Protocol) is the protocol that communicates metadata about the call. The format is very flexible and allows you to add any metadata you want. It does not stipulate any rules around latency or reliability, but gives you the tools to implement them.
 - **RTCP** also handles packet loss and gives you the tools to implement congestion control. It gives you the **in-band** bi-directional communication necessary to respond to **changing network conditions**.

 <style>
    li {
        font-size: 1rem;
        line-height: 1rem;
        @apply pt-4;
    }
</style>

<!-- 
WebRTC allows you to send and receive an unlimited amount of audio and video streams. You can add and remove these streams at anytime during a call. These streams could all be independent, or they could be bundled together! You could send a video feed of your desktop, and then include audio and video from your webcam.

RTP (Real-time Transport Protocol) is the protocol that carries the media. It was designed to allow for real-time delivery of video. It does not stipulate any rules around latency or reliability, but gives you the tools to implement them. RTP gives you streams, so you can run multiple media feeds over one connection. It also gives you the timing and ordering information you need to feed a media pipeline.

RTCP (RTP Control Protocol) is the protocol that communicates metadata about the call. The format is very flexible and allows you to add any metadata you want. This is used to communicate statistics about the call. It is also used to handle packet loss and to implement congestion control. It gives you the bi-directional communication necessary to respond to changing network conditions.
-->

---
layout: default
---

# Dealing with unpredictable networks
 - Real-life networks are **unpredictable** and **unreliable** include packet loss; **RTCP** helps mitigate this. 
 - **RTCP** allows for **PLI** (Picture Loss Indication) to be sent whenever the decoder is unable to decode a partial frame; either due to packet loss or if the decoder crashed. It requests a full key frame from the sender.
 - If only a single RTP packet should be retransmitted, RTCP can send **NACK**s with the SSRC (to identify the stream) and the packet's sequence number to the sender. If the sender does not have this RTP packet available to re-send, it ignores this message.
 - **RTCP** also includes sender and receiver reports. These are used to report statistics about the actual number of packets received and jitter. These can further be used for diagnostics and congestion control.
 - There are two primary objectives for these protocols:
   - Estimate the available bandwidth (in each direction) supported by the network.
   - Communicate network characteristics between sender and receiver.


<!-- 

Networks are unpredictable and unreliable. Bandwidth availability can change multiple times throughout a session. It is not uncommon to see available bandwidth change dramatically (orders of magnitude) within a second.

RTP/RTCP runs over all types of different networks, and as a result, it’s common for some communication to be dropped on its way from the sender to the receiver. Being built on top of UDP, there is no built-in transport layer mechanism for packet retransmission, let alone handling congestion control.

The main idea is to adjust encoding bitrate based on predicted, current, and future available network bandwidth. This ensures that video and audio signal of the best possible quality is transmitted, and the connection does not get dropped because of network congestion. Heuristics that model the network behavior and tries to predict it is known as Bandwidth estimation.
 -->

---
layout: default
---

# Adaptive Bitrate and Bandwidth Estimation

- **REMB** - Receiver Estimated Maximum Bitrate; the **sender** receives bandwidth estimation from the **receiver**, sets encoder bitrate to the received value. Doesn't work very well in practice.
- **GCC** - Google Congestion Control; used for accurate bandwidth estimation.
- **TWCC** - Transport Wide Congestion Control; the **receiver** lets the **sender** know the arrival time of each packet. This is enough information for the sender to measure inter-packet arrival delay variation, as well as identifying which packets were dropped or arrived too late to contribute to the audio/video feed. With this data being exchanged frequently, the sender is able to quickly adjust to changing network conditions and vary its output bandwidth using an algorithm such as **GCC**.

---
layout: center
class: text-center
---

# Learn More

[RFCs](https://www.w3.org/groups/wg/webrtc/publications)
