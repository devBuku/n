
## What is the problem with P2P?

![[Drawing 2026-10-28 19.53.36.excalidraw]]


- SFU (Selective Forwarding Unit): A server receives media streams from participants and selectively forwards them to other participants without usually mixing or transcoding the media. This reduces each participant's upload burden, making larger calls more practical.
- Advantages of using a server in between:
	- Stopping people's video on bad internet connection
	- Downscaling people's videos when not in focus (for example in google meet when a user's video in not pinned it's resolution is low and when pinned the quality improves)
	- How does it transcodes from 720p to 360p on the fly though?
		- Simulcast
		- SFU never encodes or transcodes videos it just forwards videos
		- The user itself sends multiple qualities videos to the SFU
		- Then the receiver's browser can ask the SFU server for the particular resolution of video
	- Receiving video in paginated manner (for e.g. zoom)
- ==The disadvantage is that we have to receive everyone's audio present on the call we can do pagination on video we cannot do pagination on audio (this leads to audio crackling)==

## Distributed SFU Architecture
![[Drawing 2026-10-28 20.38.46.excalidraw]]

# Interview Questions

1. Is an SFU and a TURN server the same?
    - No, they are different technologies. One can run a TURN server using something like `coturn`, while an SFU can be implemented using a project like `mediasoup`.
    - **SFU**: Its purpose is to forward media between participants in a call.
    - **TURN server**: Its purpose is to relay network traffic when a direct connection cannot be established.
    - Sometimes, we can even use a TURN server to relay media traffic between a client and the SFU server.
    - **They can work together: If a client cannot establish a direct connection to the SFU, its WebRTC connection can use a TURN server as a relay**
