#protocol 
#web 
#UDP 

Motive
- as we see with [[TCP]] still the data is ordered as in packet form. so there arises a problem upon giving multiple requests via [[HTTP 2]] streams, at the end they are transported as packets. So if a packet is lost, all the upcoming packets are invalidating and completely blocking further requests.

What it brings
- I uses [[QUIC]] a protocol based on [[UDP]], which brings the streamlining process in the transport level.


Use cases
- mainly useful in the scenario in the need of multiple network switches ( which is seamless with QUIC).


CONS
- No kernel level implementation (just [[UDP]] there)
- Parsing computation need