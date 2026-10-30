1. maintaining a queue for the users who want to join a call
2. need to make sure same users must not join the call again (can match after sometime not instantly to each other)
3. the call is peer to peer so we need to create a new webrtc connection everytime
4. whenever we know that two people has been matched with each other
	1. The server send user1 to send the sdp (ip, protocol etc) to receive video
	2. User1 sends that to the server
	3. Server sends it back to user2
	4. user2 be like "ok, this is where i will find user1", this my ip and everything please send it to user1
	5. Now they can talk peer to peer even after the server crashes because ip is exchanged

# Doubt
1. What is a Map? Also what is Map in javascript specifically