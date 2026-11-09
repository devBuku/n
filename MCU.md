
## Multipoint Control Unit (MCU)

- It mixes audio/video together on the server before forwarding the result to participants.
- This means it needs to:
    - Decode the audio/video (using something like FFmpeg) this is expensive.
    - Mix them (create a video canvas and mix the audio into a single stream).
    - Encode the merged streams and send them to everyone.

> Another optimization is to take the 3 **loudest audio streams, mix them, and send the merged audio to everyone.** This is for audio. We can mix video too, but we don't have to.
  This is a possible optimization called ==active-speaker selection== or ==audio mixing based on active speakers.== The _exact number of streams selected depends on the implementation_; it isn't a universal rule.

- Real World use case: 
	- Cloud Recording (a single mp4 i being created all the audio and video are getting merged)
		- Example: Zoom call recording

[[webrtc]]
[[cohort]]