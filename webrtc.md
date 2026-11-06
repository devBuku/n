
- WebRTC is the main technology that lets you do **real-time media communication** from ==inside a browser.==
- It is a udp based protocol (which means that packets can be lost in between which is fine in this communication, losing packet will just cause a jitter )
- UDP but can fallback to TCP
- **Sub-second live call** = the time taken for audio to travel from one side of the call to the other is less than 1 second.
- **HLS (HTTP Live Streaming)** = probably used in YouTube Live streams. It usually has around 10–30 seconds of delay, or a few seconds in low-latency mode. WebRTC is used in things like Google Meet calls. (Zoom mostly uses its own media stack, not WebRTC.)
- **HLS** = 10 seconds or more of delay → cricket matches → can prioritize high-quality, stable playback.
- **WebRTC** = low delay → useful for interactive calls, but can require more infrastructure and bandwidth costs. Playback can be choppy if the network is unstable.


# jargon

[WebRTC For The Curious](https://webrtcforthecurious.com/)
> WebRTC For The Curious is an open-source book created by WebRTC implementers to share their hard-earned knowledge with the world. It's written for those who are always looking for more and don't settle for abstraction.

1. WebRTC supports **peer-to-peer (P2P)** communication. This means that media can be sent directly to the other person without going through a central server. However, sometimes the media has to go through a relay server (TURN).
   - Is WebRTC a P2P protocol? (yes, but direct communication is not always possible.)

   ![[Screenshot From 2026-10-28 11-21-13.png]]

2. One reason for using P2P is that relaying all the video through a server can get expensive very quickly.

3. **Signalling server**: Both browsers need to exchange information about how to connect before they can start talking to each other. A **signalling server** is used for that. It's usually a WebSocket server, but it can use other methods, such as HTTP. It's like exchanging phone numbers so you can talk.
   
4. **STUN (Session Traversal Utilities for NAT (Network Address Translation))**: helps you discover your public-facing IP address and port. ==It shows you how the world sees you==.
   - Why do we need a STUN server? Because we need to discover a reachable address to share with the other peer.
   - Why can't we use the signalling server to get the address? The signalling server sees the network address of the connection made to it, but that doesn't necessarily tell us all the possible ways to reach our machine. There are multiple possible ways to connect to a system using public and private addresses and ports. These are represented by **ICE candidates**.
   - [Trickle All the ICE for us](https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/)
   - `stun:stun.l.google.com:19302` = a public STUN server provided by Google.

5. **ICE (Interactive Connectivity Establishment) candidates**: potential networking endpoints that WebRTC uses to establish a connection between peers. These can include local addresses, public-facing addresses discovered using STUN, and relay addresses provided by TURN.

6. If two friends are trying to connect to each other on the same ==hostel Wi-Fi==, they might be able to connect via their ==private IP addresses==, if the network allows it.

7. If two people from ==different countries== are trying to connect, they may connect using their ==public-facing IP addresses and ports==, if their NATs and firewalls allow it. Otherwise, they may need a TURN relay.

8. **TURN server**: relays media when the peers cannot communicate directly (for example, because their NAT configurations or firewalls prevent a direct connection).
	> [coturn](https://github.com/coturn/coturn)
	coturn is a free open source implementation of TURN and STUN Server. The TURN Server is a VoIP media traffic NAT traversal server and gateway.

9. **Offer**: the ==first browser== (the one initiating the connection) sends its session description (SDP) to the other side. It describes the media capabilities and connection details. ICE candidates may be included, depending on when they are gathered.

10. **Answer**: the ==other side== sends back its own SDP, describing its side of the connection.

11. **SDP (Session Description Protocol)**:
    - A block of text that describes things such as:
        - Media types and the codecs they support.
        - Connection details, including ICE-related information when included.
        - Other parameters needed to negotiate the session.
    - This is what gets sent in the offer and received in the answer. ICE candidates can also be sent separately through signalling (this is called trickle ICE).

12. **RTCPeerConnection (peer connection)**: This is a class that the browser provides, which lets you create offers/answers, access the SDP, and send/receive media. It hides much of the complexity of WebRTC from the developer.

# Connecting the two sides

1. Browser 1 creates an ==RTCPeerConnection==.
2. Browser 1 creates an offer.
3. Browser 1 sets the ==local description== to the offer.
4. Browser 1 sends the offer to the other side through the ==signalling server==.
5. Browser 2 receives the ==offer== from the ==signalling server==.
6. Browser 2 sets the ==remote description== to the offer.
7. Browser 2 creates an answer.
8. Browser 2 sets the ==local description== to the answer.
9. Browser 2 sends the answer to the other side through the ==signalling server==.
10. Browser 1 receives the ==answer== and sets the ==remote description==.
> This is just to establish the p2p connection b/w two parties

To actually send media, we have to:
1. Ask for camera / mic permissions
2. Get the audio and video streams
3. Call **addTrack** on the pc (peer connection)
4. This would trigger a **onTrack** callback on the other side

![[webrtc_code_diagram_comparison_cohort.png]]


# Doubts

1. So does the signalling server already know my address as well as the other machine's address?
   - I guess the answer here is: I am sending a request to the signalling server, and the other person is also connecting to it, so yes, the signalling server can see the network address of each connection. But this doesn't necessarily mean it knows every possible address that can be used to connect to each machine. That's one reason STUN and ICE are useful.

# btw

![[jsfiddle_permission_request.png]]

> The JSFiddle website asked me to whitelist it. (That means scripts running on the website can execute in my browser, subject to the browser's security restrictions and permissions. Depending on the situation, they may be able to access cookies and other data available to that site.) While Harkirat was showing the WebRTC example, he also showed `about:webrtc` (Firefox's WebRTC debugging page). Because of Firefox and uBlock Origin's content blocking, I got this pop-up, but it wouldn't appear on Chrome.

**Whitelisting a website in uBlock Origin generally disables filtering for that site, depending on the configuration. It doesn't disable the browser's same-origin policy or automatically grant access to cookies belonging to other websites.**

# Other architectures
[[SFU]]
[[MCU]]
[[cohort]]