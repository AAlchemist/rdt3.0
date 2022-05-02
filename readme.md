### Global variables
1.	senderState: specifies the server’s states ( either WAIT_FOR_CALL or WAIT_FOR_ACK)
2.	lastPacket: a copy of the last transmitted packet by the sender for possible retransmission.
3.	lastRcv: a copy of the last transmitted packet for the receiver to send NAK by sending ACK with acknum=lastRcv.seqnum.
### Functions
1.	getChecksum(): calculates and returns the checksum of the given packet. (treat each character in the payload as if it were an 8 bit int and add them together).
2.	A_output(): sends messages from the upper layer. 
a)	Ensure that the sender is in WAIT_FOR_CALL state.
b)	Create a packet containing the message to be sent, along with the seqnum and checksum.
c)	Send the packet to layer 3, start the countdown timer, and set the senderState to WAIT_FOR_ACK.
3.	A_input(): receives packet from the receiver.
a)	Ensure that the sender is in WAIT_FOR_ACK state.
b)	If the packet is neither corrupted(can be detected by checksum) nor contains NAK (acknum=1-lastPacket.seqnum), then stop the countdown timer, update the senderState and lastPacket.
c)	Otherwise, just discard the packet.
4.	A_timerinterrupt(): handle the sender’s timeout event.
a)	Ensure that the sender is in WAIT_FOR_ACK state.
b)	Retransmit the last packet (lastPacket).
c)	Reset the countdown timer.
5.	B_input(): when a packet arrives at the receiver.
a)	If the packet is neither corrupted nor contains unexpected seq number, then send an ACK packet back to the sender, and deliver the packet to the upper layer.
b)	Otherwise, send an NAK packet back to the sender. 

### References
https://slideplayer.com/slide/15347345/
![](https://player.slideplayer.com/92/15347345/slides/slide_5.jpg)
![](https://player.slideplayer.com/92/15347345/slides/slide_6.jpg)