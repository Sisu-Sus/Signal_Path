
This is a document on studying signal path from coax to user management.
If you expect academic writing over integral information you are in the wrong place.
If you are an anxious impatient reader like me skip to the WHAT WE LEARNED sections for TLDR

Simplified signal path

[coax] -> [modem] -> [ethernet] -> [nic] -> [kernel driver] -> [network stack] ->

[application layer] -> [user/system response]


--------------------------------------------------------

A. Coax Cable Transmission

General Knowledge:

coaxial cable carries modulated analog radio frequency signals
(continuous waves that encode digital data)

this is used primarily by Data Over Cable Service Interface Specification (DOCSIS)
for cable internet.

---

DOCSIS * The data I could find reference DOCSIS4.0 But did not give pertininent information on it *
	* Current working DOCSIS beneath DOCSIS4.0 is 3.1 *

Data Over Cable Interface Specification is an international telecommunications
standard that permits the addition of high-bandwidth data transfer to an existing
cable television(CATV) system.

Physical Layer
	Channel width:

		-Downstream:
		 All versions of DOCSIS earlier than 3.1 use either 6 MHz channels
		 (North America) OR 8 MHz (EuroDOCSIS). DOCSIS 3.1 uses channel bandwidths up
		 to 192 MHz.

		-Upstream:
		 DOCSIS 1.0/1.1 specifies channel widths between 200Khz and 3.2 MHz.
		 DOCSIS 2.0 & 3.0 specify 5.4MHz, but can use the earlier, narrower channel
		 widths for backward compatability. DOCSIS 3.1 uses channel bandwidths of up to
		 96MHz in the upstream


	Modulation: * The process of converting digital into analog for transmission *
			* QAM - Quadtrature Amplitude Modulation *

		-Downstream:
		 All versions of DOCSIS prior to 3.1 specify that 64-level or 256-
		 level QAM (64-QAM or 256-QAM) be used for modulation of downstream data using
		 the correct level of standard for operation(framework, coding and distribution).

		 3.1 adds 16-QAM, 128-QAM, 512-QAM, 1024-QAM, and 4094-QAM, with optional
		 support of 8192-QAM/16384-QAM.

		-Upstream:
		 Upstream data uses QPSK(Qaudrature Phase-Shift Keying) or 16-level QAM for DOCSIS1.x
		 While QPSK, 8-QAM, 16-QAM, 32-QAM, and 64-QAM are used for DOCSIS 2.0 & 3.0.
		 DOCSIS 2.0 & 3.0 also support 128-QAM with trellis coded modulation in S-CDMA mode.

		 DOCSIS 3.1 supports daya modulations from QPSK up to 1024-QAM, with optional
		 support for 2048-QAM and 4096-QAM.

Data Link Layer
	DOCSIS employs a mixture of access methods for upstream transmissions, specifically time-
	division mutliple access (TDMA) for DOCSIS 1.0/1.1 and both TDMA and S-CDMA for DOCSIS
	2.0 & 3.0 with a limited use of contention for bandwidth reservation requests. In TDMA,
	a cable modem requests a time to transmit and the CMTS grants it an available time slot.

	For DOCSIS 1.1 and above, the data layer also includes extensive quality of service (QOS)
	features that help to efficiently support applications that have specific traffic
	requirements such as low latency (VoiP).
	DOCSIS 3.0 feature channel bonding, which enables multiple downstream and upstream
	channels to be used together at the same time by a single subscriber.

Throughput
	Bandwidth is shared among users of an HFC, within service groups which are groups of
	customers that share RF channels.

	The first three versions of DOCSIS standard support a downstream throughput with 256-QAM
	of up to 42.88Mbit/s per 6MHz channel(Approximately 38Mbit/s after overhead), or
	55.62 Mbit/s per 8 MHz channel for EURODOCSIS(approximately 27Mbit/s after overhead).
	The upstream throughput is possible at 30.72Mbit/s per 6.4 MHz channel (approximately
	27 Mbit/s after overhead), or 10.24 Mbit/s per 3.2 MHz channel (approx. 9Mbit/s after
	overhead).

	DOCSIS 3.1 supports a downstream throughput with 4096-QAM and 35 kHz subcarrier spacing
	of up to 1.89Gbit's per 192 MHz OFDM channel. The upstream throughput possible is 0.94
	Gbit/s per 96 MHz OFDMA channel

Network Layer
	DOCSIS modems are managed via an IP address
	The DOCSIS 2.0 + IPV6 specification allowed support on DOCSIS 2.0 modems via firmware update
	DOCSIS 3.0 added management over IPV6

Equipment
	DOCSIS consists of two primary componemts: a cable modem and a cable modem termination
	system (CMTS) located at the CATV headend

Security
	DOCSIS uses media access control (MAC) in its Baseline Privacy Interface (BPI) specs.
	DOCSIS 1.0 used the initial BPI. It was later improved with BPI+ used in DOCSIS versions
	1.1 and 2.0. A number of enhancements have been made to the BPI in DOCSIS 3.0 and the
	specification was actually just renamed SEC for security.

	SEC intends to stop cable users from listening to each other by encrypting data flows
	between CMTS and the cable modem. BPI and BPI+ use 56-bit Data Encryption Standard (DES),
	while SEC also adds support for 128-bit Advanced Ecnryption Standard (AES). The AES key
	is protected by a 1024-bit RSA key.

	BPI+ extends its protection by implementing digital certificate based authentication to
	its key exhange protocol.It uses a Public Key Infastructure(PKI) based on digital
	ticket authorities (CAs) of the certification testers. Typically the cable service
	operator adds the modems MAC manually to the service users account, allowing access
	only to a cable modem that can attest to that MAC address using a valid certificate
	issued via the PKI. Early BPI (ANSI/SCTE22-2) had underlying key managemenet issues
	where the protocol did not authenticate the user's cable modem.

	Successful attacks often occur when the CMTS is configured for backward compatibility
	with early pre-standard DOCSIS 1.1 modems. These modems were "Software Upgradable" in
	the field but did not include valid DOCSIS or EURODOCSIS root CAs.

WHAT WE LEARNED:
A wave of data is sent using RF and modulated using a protocol deterministic on the DOCSIS version.
One could easily have a chart handy for quick access to specifics if confused.
Specifics:
Physical band width, frequency, sec, frameworks are all version specific. DOCSIS << 1.1 is gold

			=== Basic Chart ===
=================================================================.
DOCSIS Version | Production Date | Max Downstream | Max Upstream |
-----------------------------------------------------------------.
     1.0       |       1997	 |                |              |
---------------|-----------------|                |   10 Mbit/s  |
     1.1       |       2001      |    40 Mbit/s   |		 |
---------------|-----------------|                |--------------|
     2.0       |       2002      |   		  |   30 Mbit/s  |
---------------|-----------------|----------------|--------------|
     3.0       |       2006      |    1 Gbit/s    |  200 Mbit/s  |
---------------|-----------------|----------------|--------------|
     3.1       |       2013      |		  |  1 -2 Gbit/s |
---------------|-----------------|   10 Gbit/s    |--------------|
     4.0       |       2017      |                |    6 Gbit/s  |
-----------------------------------------------------------------^

source:wikipedia
---
QAM: Quadrature Amplitude Modulation

	QAM is a method of combining two amplitude modulation (AM) signals into a single channel.
	This scheme helps double the channels effective bandwidth. QAM is also used with pulse AM
	in digital systems.

How does QAM work?

	QAM is used to increase spectrum efficiency. It accomplishes this task by utlizing both
	phase and amplitude components to modulate or alter the waveform. The following example
	consists of two signals known as carrier waves.

	One signal is call the I signal and the other is the Q signal. This is also why QAM
	is referred to as IQ modulation. Mathematically on of the signals can be represented
	with a sine wave and the other by a cosine wave. These carrier waves each have the same
	frequency but differ in phases by 90 degrees, or one quarter cycle. Also, they are 90
	degrees out of phase with each other, which is why they are orthogonal or in quadtrature
	with each other.

	The two signals are represented by cos(θ) = sin (θ-90°)

	The two modulated carriers combine at the source -- within a QAM waveform -- for
	transmission. The modulator works like a translator. Its job is to encode, or translate
	digital packets into an analog signal, thus ensuring transfers. The combined carrier
	signals are then transmetted over the same channel to the demodulator(receiver)

	At the destination, the demodulator seperates the carrier waves and extracts the data
	from each. A low-pass filter -- an electronic circuit that surpresses higher
	frequencies -- is used to extract the in-phase and quatrature components of the signal.
	Then the data is incorporated into the original modulating information.

Analog QAM vs Digital QAM

Analog QAM:
	Analog QAM enables carriers to transmit multiple analog signals. For example
	QAM is used in Phase Alternating Line (PAL) and National Television
	Systems Committee (NTSC). In this case the different channels QAM provides
	enables the signal to carry components of color or chroma data

	A system known as Compatible QUAM is found in AM stereo brodcasting applications
	in the US and other countries. This scenario involves two distinct modulation
	stages: conventional AM and a compatible quadtrature phase modulation. The
	different channels enable the required two channels for stereo to be carried by
	a single carrier. It uses dedicated circuitry to encode a stereo seperation
	signal that is compatible with older receivers.

Digital QAM:
	Version of digital QAM are often called quantized QAM. The are built into most
	radio commuinications systems that use data. For example, radio communications
	technologies ranging from 4G and 5G cellular systems to use different types of
	QAM. As the field evolves, the number of QAM systems used in radio communications
	is likely to increase.

WIFI and QAM
	QAM continues to be used with the latest WIFI standard currently WIFI7. This is known
	as 4096-QAM or 4kQAM with a better security and uptime than its former versions.
	This is crucial for devices that operate a 4k streaming application for example.
	WIFI 6holds a 1024-QAM standard how ever it is more prone to intereference
	then the former 256-QAM that is the standard for WIFI5.

Cable TV and QAM
	QAM systems are popular in the cable television industry. In the US 64-QAM and 256-QAM
	are commonly used for digital TV series. Muliple System Operators (MSO) and other
	network operators use QAM to deliver services as it provides formatting services in
	hubs where signals are processed and distributed over the cable operators network.
	Once in a residence the cable modems and set-tops convert QAM signals back into their
	original format.

WHAT WE LEARNED:

	QAM maps binary data to complex waveforms by AMPLITUDE: how tall the wave is, and PHASE:
	where in the cycle it is. This symbol represents a STATE of binary (0000000) which
	is translated in real time into machine langauge.
               === Basic Chart ===
_____________________________________________________
|           | 802.11ac |   802.11ax   |   802.11be   |
=====================================================.
| Frequency |  5 Ghz   |2.4 GHz, 5 Ghz|  2.4/5/6 GHz |
-----------------------------------------------------.
|    Bits   |    4     |       8      |       16     |
-----------------------------------------------------.
|  Max Rate | 6.9Gbps  |    9.6 Gbps  |    46 Gpbs   |
-----------------------------------------------------.
|   AP Cap. |   OFDM   |     OFDM     |    OFDMA     |
-----------------------------------------------------.
|    QAM    | 256-QAM  |   1024-QAM   |   4096-QAM   |
-----------------------------------------------------.
|  MU-MIMO  | Downlink |  UP/DOWNlink |  UP/DOWNlink |
-----------------------------------------------------*
source:techtarget
---

Radio Frequency Bands
	The radio frequency spectrum (RF) is one such part of the electromagnetic spectrum
	that overlaps our sub-THz range at its lower end. Accordingly, Electromagnetic waves in
	this frequency range are called radio requency bands or simply 'radio waves'. RF bands
	spread in the range between 30kHz and 300GHz (alternative point of view offers 2kHz -
	300GHz). All known transmission systems are operated in the RF spectrum range.

	Honestly researching this you are better to just go look at the millions of charts on
	google to get a better idea. Its actually fairly simple, here is a basic chart established
	as the standard among professionals. The difference in charts is mostly naming conventions
	per industry standard.

__________________________________________________________
|   Band  | Frequency Range  ||  Band   | Frequency Range |
|=========|==================||=========|=================.
| R band  | 1.70 to 2.60GHz  || K band  | 18.0 to 26.5GHz |
|---------|------------------||---------|-----------------*
| D band  | 2.20 to 2.20GHz  || Ka band |  26.5 to 40GHz  |
|---------|------------------||---------|-----------------*
| S band  | 2.60 to 3.95GHz  || Q band  |   33 to 50GHz   |
|---------|------------------||---------|-----------------*
| E band  | 3.30 to 4.90GHz  || U band  |   40 to 60GHz   |
|---------|------------------||---------|-----------------*
| G band  | 3.95 to 5.85GHz  || V band  |   40 to 75GHz   |
|---------|------------------||---------|-----------------*
| F band  | 4.90 to 7.05GHz  || E band  |   60 to 90GHz   |
|---------|------------------||---------|-----------------*
| C band  | 5.85 to 8.20GHz  || W band  |   75 to 110GHz  |
|---------|------------------||---------|-----------------*
| H band  | 7.05 to 10.10GHz || F band  |  90 to 140 GHz  |
|---------|------------------||---------|-----------------*
| X band  | 8.2 to 12.5GHz   || D band  |  110 to 170GHz  |
|---------|------------------||---------|-----------------*
| Ku band | 12.4 to 18.0GHz  || Y band  |  325 to 500GHz  |
----------------------------------------------------------*

WHAT WE LEARNED:
	RF bands come in a multitude of frequencies. Each band has a different naming convention
	dependant on frequencies used within the band. The RF spectrum overlaps with the
	electromagnetic spectrum.
source:terasense
---

CMTS(Cable Modem Termination System): The ISPS head end that talks to your modem

	A cable modem termination system or CTMS for short can also be referred to as a
	CMTS Edge Router. Its a piece of equipment typically located in a cable company's head
	end or hubsite, which is used to provide data services such as cable internet or VOIP.
	CMTS provides similar functions to a DSLM in a digital subscriber line or optical
	optical line termination in a passive optical network.


Connections:
	In order to provide highspeed data services, a cable company will connect its headend
	to the internet via high capacity data links. These head ends will inturn communicate
	with subscribers cable modems. Different CMTS are capable of serving different cable modem
	population sizes -- ranging from 4000 modems to over 150 000. A given headend may have
	between 1 - 12 CMTS.

	A service group is a group of customers that share the same RF channels and thusi bandwidth.
	A CMTS has seperate RF interfaces and connectors for the downlink and uplink signals. The
	RF/COAX interface carry RG signals to and from coaxial trunks connected to the customers
	home modem. Ever connector has a finite number channels it can carry, such as 16 downstream
	and 4 channels upstream per connector. A service group may serve up to 500 households.
	Channels are regrouped at the headend or distritution hub and serviced by CMTSs and other
	equipment such as Edge QAMs.

	The RF signals from a CMTS are connected via coaxial cable to headend RF management modules
	for splitting and combining, with other equipement such as other CMTSs so that serveral
	CMTS can service one service group, and then to an optics platform or headend platform,
	which has transmitter and receiver modules that turn RF signals into light pulses for
	delivery over fiber optics through a hybrid fiber-coaxial network. Most CMTS will have
	an ethernet port in order to bridge or route traffic such as your own modem and or router.

	CMTS typically only carry IP traffic. Traffic destined for the cable modem from the
	internet, or downstream traffic, is carried in IP packets encapsulated according to the
	DOCSIS standard. These packets are then modulated onto a TV channel using either
	64-QAM, or 256-QAM to give us 8-bit architecture. (If you're new this is where it
	clicked for me).

	Upstream data (data from the headend or internet) is carred on data streams that are
	encapsulated DOCSIS frames modulated with QPSK, 16-QAM, 32-QAM, 64-QAM or 128-QAM
	using Time Division Multiple Access(TDMA), Advanced TDMA(ATDMA) or Synchronous Code
	Division Multiple Access(S-CDMA) frequenzy sharing mechanisms. This is usually done at
	the subband or return portion of the cable TV spectrum ( also known as T channels ),
	a much lower part of the frequency spectrum than the downstream signal, usually 5-42MHz
	in DOCSIS2.0 or 5-65MHz in EuroDOCSIS.


	A typical CMTS allows a subscribers computer to obtain an IP address by forwarding DHCP
	requests to the relevant servers. The DHCP returns for the most part, what looks like a
	typical response including an assigned IP address for the computer, gateway/router
	addresses to use, DNS servers etc.

	The CMTS may also implement some basic filtering to protect against unauthorized users
	and various attacks. Traffic shapingis sometimes performed to prioritize application
	traffic, perhaps based upan subscribed plan or download usage. This is also to provide
	guaranteed Quality of Service(QoS) for the cable operator's own PacketCable-based VOIP
	service. However, the function of traffic shaping is more likely done by a Cable Modem
	or policy traffic switch. A CMTS may also act as a bridge or router.

	A customer's cable modem cannot comunnicate directly directly with other modemsn on
	the line. In general, cable modem traffic is routed to other cable modems or to the
	Internet through a series of CMTSs and traditional routers. However, a route could
	conceivably pass through a single CMTS.

	A Converged Cable Access Platform(CCAP) combines CMTS and EDGE QAM functionality in a
	single device so that it can provide both data(internet) with CMTS functionality,
	and TV channels with Edge-QAM functionality. Edge-QAM converts video send via IP or
	otherwise, into a QAM signal for delivery over a cable network. EDGE QAMs are
	normally stand-alone devices placed at the "Edge" of a network. They can also be
	connected to a CMTS core, to make an M-CMTS system which is more scalable.

Architecures
	A CMTS can be broken down into several different architectures, Integrated CMTS (I-CMTS),
	virtual CMTS(vCMTS), and remote CMTS. An I-CMTS incorporates intoa single unit all components
	neccassary for its operation. There are both pro's and cons to each type of architecture.

Modular CMTS (M-CMTS)
	In a M-CMTS solution the architecture of an I-CMTS is broken up into two components. The
	first part is the Physical Downstream component(PHY) which is known as the Edge QAM (EQAM).
	The second part is IP networking and DOCSIS MAC Component which is referred to as the
	M-CMTS Core. There are also several new protocols and components introduced with this type
	of architecture. One if the DOCSIS Timing Interface, which provides a reference
	frequency between EQAM and M-CMTS Core via a DTI Server. The Second is the Downstream
	External PHY Interface(DEPI). The DEPI protocol controls the delivery of DOCSIS frames
	from the M-CMTS Core to the EQAM devices. Some of the challenges that entail an M-CMTS
	architecture is that it is extremely scalable to larger numbers of downstream channels.

Virtual CMTS
	Virtual CCAPs(VCAPPs) or virtual CMTS(vCMTS) are implemented on commercial off the shelf
	x86-based servers with specialized softeware and can be used to increase service capacity
	without purchasing new CMTS/CCAP chassis or add features to the CMTS/CCAP more quickly.

Remote CMTS
	Remote CMTS/Remote CCAP moves all CMTS/CCAP functionality to the outside plant, in stark
	contrast to conventional CMTSs or CCAPs which are installed at a service provider
	location.

WHAT WE LEARNED:
	A cable modem access system or CMTS for short is a system used by providers to forward
	requests through DHCP to the relevant servers in order to provide their service. Typically
	used to assign IP to subscribers, and handle IP traffic, the architectures can very per setup.
	Architectures are Integrated, Modular, and Remote.

source:wikipedia
---

HARDWARE
Coaxial cable: Carries the RF signal from ISP to your modem
Modem: Modulator-Demodulator - converts the RF signal to digital packets over ethernet


------------------------------------------------------------


B. Modem to Ethernet (Layer 1 & 2)

General Knowledge:

The modem demodulates the signal and extracts digital frames
Converts to ethernet frames(Layer 2) and sends via copper Ethernet(RJ45, twisted pair)


---
Ethernet frame:
	In computer networking, an Ethernet frame is a data link layer protocol data unit and uses
	the underlying Ethernet physical layer transport mechanisms. In other words, a data unit
	on an Ethernet link transports an Ethernet frame as its payload.

	An ethernet frame is preceded by a preamble and Start From Delimter (SFD), which are both
	part of the Ethernet packet at the physical layer. Each Ethernet frame starts with an
	Ethernet header, which contains destination and source MAC addresses as its first two fields.
	The middle section of the frame is the payload data including any headers for other protocols
	such as IP. The frame ends with a Frame Check Sequence (FCS) which is a 32-bit Cyclic
	Redundancy Check -- a type of checksum that detects errors by performing polynomial-based
	calculations on the frame's contents. This helps ensure data integrity during transmission.
							__________________________
	*octets = bytes*		      	       |	Octets		  |
							--------------------------
	Ethernet packet					Length | Layer 2 | Layer 3
		|				       (octets)|	 |
		|-> Preamble            	           7   | Not in  |
		|-> SFD 				   1   | Frame   |
		|--------------------------------------------------------|
		|-> Destination MAC Address		   6   |	 | 72 - 1530 octets
		|-> MAC Source				   6   |         |
		|-> 802.1Q tag (optional)       	  (4)  |         |
		|-> EtherType(E2) or length(802.3)         2   |   64 -  |
		|-> Payload		             42 - 1500 |   1522  |
		~				          -    |         |
		|-> FCS				          4    |         |
		|----------------------------------------------------------------------
		|-> Interpacket			         12    | Not in  |    12 octets
		     Gap (IPG)			                 frame

Structure
	A data packet on the wire with the frame as its payload consist of binary data. Ethernet
	transmits data with the most-significant octet (byte) first; within each octect, however,
	the least-significant bit is transmitted first.

	The internal structure of an Ethernet frame is specified in IEEE 802.3. The table below
	shows the complete Ethernet packet and the frame inside, as transmitted, for the payload
	size up to the MTU of 1500 octets. Some implementations of Gigabit Ethernet and other
	higher speed variants of Ethernet support larger frames, known as jumbo frames.

Ethernet Packet - Physical layer
	Preamble and start frame delimiter
		
	An ethernet packet starts with a seven-octet (56-bit) preamble and one-octet(8-bit) start
	frame delimiter(SFD). The preamble bit values alternate 1 and 0, allowing receivers to
	synchronize their clock at the bit-level with the transmitter. The preamble is followed
	by the SFD which ends with a 1 or 0, to break the bit pattern of the preamble and signal
	the start of the actual frame.

	Physical layer transceiver circuitry(PHY) is required to connect the Ethernet MAC to the
	physical medium. The connection between a PHY and MAC is the independant of the physical
	medium a bus from the media-independant interface(MII) family. I feel MII is a subject
	in itself to study for sure. The preamble and SFD representation depends on the width of bus:


Representation 			56-bit (7-byte) Preamble 			       | SFD byte
===================================================================================================
A)   10101010    10101010    10101010    10101010    10101010    10101010    10101010  | 10101011
----------------------------------------------------------------------------------------------------
B)     85 	    85 	        85 	    85    	85 	    85  	85     |   213
----------------------------------------------------------------------------------------------------
C)    0x55 	   0x55        0x55        0x55        0x55 	   0x55        0x55    |   0xD5
----------------------------------------------------------------------------------------------------
D)   0x5|0x5 	  0x5|0x5     0x5|0x5 	  0x5|0x5     0x5|0x5 	  0x5|0x5     0x5|0x5  |  0x5|0xD 
----------------------------------------------------------------------------------------------------

A* Uncoded on-the-wire bit pattern transmitted from left to right ( used by Ethernet variants
 	transmitting serial bits instead of larger symbols)

B* Decimal in Ethernet Lsb-first ordering

C* Hexadecimal LSb-first bytes for 8 bit wide busses ( GMII bus for Gigabit Ethernet transceivers)

D* Hexadecimal Lsb-first nibbles for 4-bit wide busses ( MII bus for Fast Ethernet or RGMII for
  	gigabit transceivers)


  *A nibble or nybble to match byte, is a unit of information that is an aggregation of 4 bits.*

   *Lsb-first means Least Significant Bit First*

Frame - Data link layer
	Header:
		The header features destination and source MAC addresses ( each six octets in length),
		the Ethertype field and, optionally, an IEEE 802.1Q tag or IEEE 802.1ad tag
		
		The EtherType field is two octets long and ti can be used for two different purposes.
		Values of 1500 and below indicate that it is used as an EtherType, to indicate 
		which protocol is encapsulated in the payload of the frame. When used as EtherType,
		the length of the frame is determined by the location of the interpacket gap and
		valid frame check sequence.

		The IEEE 802.1Q tag or IEEE 802.1ad tag, if present, is a four octet field that
		indicates virtual lan (VLAN) membership and IEEE 802.1p priority. The first two
		octets of the tag are called the Tag Protocol IDentifies (TPID) and double as the
		EtherType field indicating that the frame is either 802.1Q or 802.1ad tagged. 802.1Q
		uses a TPID of 0x88a8.

	Payload:
		Payload is a variable length field. Its minimum size is governed by a requirement for
		a minimum frame transmission of 64 octets(bytes). With the header and FCS taken into
		account, the minimum payload is 42 octets when an 802.1Q tag is present and 46 octets
		when absent. When the actual payload is less then the minimum, padding octets are
		added accordingly. IEEE standards specify a maximum payload of 1500 octets. Non-
		standard jumbo frames allow for larger payloads on networks built to support them.

	Frame Check Sequence:
		The frame check sequence (FCS) is a four octet cyclic redundancy check (CRC) that
		allows detection of corrupted data within the entire frame received on the receiver
		side. According to the standard, the frame check sequence (FCS) value is computed as 
		a function of the protected MAC frame fields: source and destination address, length/
		type field, MAC client data and padding (that is all field except the FCS).

		Per the standard, this computation is done using the left shifting CRC-32
		(polynomial = 0x4C11DB7, initial CRC = 0xFFFFFFFF, CRC is post complemented,
		verify value = 0xEDB88320) algorithm. The standard states that data is transmitted
		least significant bit first(bit 0) while the FCS is transmitted most signigicant bit
		first (bit 31). An alternative is to calculate CRC using the right shifting CRC-31
		(polynomial = 0xEDB88320, inital CRC = 0xFFFFFFFF, CRC is post complemented, verify
		value = 0x2144DF1C), which will result in a CRC that is a bit reversal of the FCS, 
		and transmit both data and the CRC least significant bit first, resulting in
		identical transmissions.

		The standard states that the receiver should calculate a new FCS as data
		is received and then compare the received FCS with the FCS the receiver
		has calculated. An alternative is to calculate a CRC on both the reveived data and
		the FCS, which will result in a fixed non-zero verify value. (The result is non-zero
		because the CRC is post complemented during CRC generation). Since the data is
		received least significant bit first, and to avoid having to buffer octets of data,
		the receiver typically uses the right shifting CRC-32. This makes the verify value
		(sometimes called magic check) 0x2144DF1C.

		However, hardware implementation of a logically right shifting CRC may use left
		shifting  Linear Feedback Shift Register (LFSR) as the basis for calculating CRC,
		reversing the bits and resultiong ina verify value of 0x38FB2284. Since complementing
		of the CRC may be performed post calculation and during transmission, what 
		remains in the hardware register is a non-complemented result, so the residue for a
		right shifting implementation would be the complement of 9x2144DF1C, and for a left
		shifting implementation, the compliment of 0x38FB2284 = 0xC704DD7B

End of frame - physical layer:
	The end of a frame is usually indicated by the end-of-data-stream symbol at physical layer
	or by loss of the carrier. Later physical laters use an explicit end of data or end of
	stream symbol or sequence to avoid ambiguity, especially where the carrior is continually
	sent between frames; an example is a Gigabit Ethernet with its 8b/10b encoding scheme that
	uses special symbols which are transmitted before and after a frame is transmitted.

Interpacket gap - physical layer
	Interpack gap (IPG) is idle time between packets. After a packet has been sent, transmitters
	are required to transmit a minimum of 96 bits (12 octets) of idle line state before
	transmitting the next packet.

Types
	There are several types of Ethernet frames:
		- Ethernet II frame, or Ethernet Version 2, or DIX frame is the most common type
		  in use today as it is often used directly by the Internet Protocol
		- Novell raw IEEE 802.3 non standard variation frame
		- IEEE 802.2 Logical Link Control(LLC) frame
		- IEEE 802.2 Subnetwork Access Protocol(SNAP) frame
	
	The different frame types have different formats and MTU values but can coexist on the same
	physical medium. Differentiation between frame types is possible based on the following:

		=== Ethernet Frame Differentiation ===
=====================================================================.
     Frame Type      | Ethertype or length | Payload start two bytes |
---------------------------------------------------------------------*
     Ethernet II     |       >= 1536       |            Any          |
---------------------|---------------------|-------------------------.
Novell raw IEEE 802.3|                     |           0xFFFF        |
---------------------|			   |-------------------------.
   IEEE 802.2 LLC    |       <= 1500       |           Other         |
---------------------|                     |-------------------------.
   IEEE 802.2 SNAP   |                     |           0xAAAA        |
---------------------------------------------------------------------*

In addition, all four Ethernet frame types may optionally contain an IEEE 802.1Q tag to idnetify what
VLAN it belongs to and its priority(quality of service). This encapsulation is defined in the IEEE
802.3ac specification and increases the maximum frame by 4 octets.

The IEEE 802.1Q tag, if present, is placed between the Source Address and the Ethertype or Length
fields. The first two octets of the tag are the Tag Protocol Identifier(TPID) value of 0x8100. This
is located in the same place as the Ethertype/Length field in untagged frames, so an Ethertype value
of 0x8100 means the frame is tagged, and the true Ethertype/Length is located after the Q-tag. The
TPID is followed by two octets containing the Tag Control Information (TCI)(The IEEE802.1p priority
and VLAN id). The Q-tag is followed by the rest of the frame, using one of the types described above.


Ethernet II
	Ethernet II framing (also known as DIX ethernet, named after DEC, Intel, and Xerox, the 
	major participants in the design), defines the two octet Ethertype field in an Ethernet
	frame, preceded by its destination and source MAC addresses, that identifies an upper
	layer protocol encapsulated by the frame data. Most notably, an Ethertype value of 0x0800
	indicates that the frame contains an IPv4 datagram, 0x0806 indicates an ARP datagram, and
	0x86DD indicates an IPv6 datagram.

	As this industry-developed standard went through a formal IEEE standardization process, the
	Ethertype field was changed to a (data) length field in the new 802.3 standard. Since the
	recipient still needs to know how to interpret the frame, the standard required an IEEE802.2
	header to follow the length and specify the type. Many years later, the 802.3x-1997 standard,
	and later versions of the 802.3 standard, formally approved of both types of framing. Ethernet
	II framing is the most commong in Ethernet local area networks, due to its simplicity and low
	overhead.

Novell raw IEEE 802.3
	Novell's raw 802.3 frame format was based on early IEEE802.3 work. Novell used this as a
	starting point to create the first implementation of its own IPX Network Protocol over
	Ethernet. They did not use any LLC header but started the IPX packer directly after the	
	length field. This does not conform to the IEEE802.3 standard, but since IPX always has FF
	as the first two octets(while in IEEE802.2 LLC that pattern is theoretically possible but
	extremely unlikely), in practice this usually coexists on the wire with out Ethernet
	implementations, with the notable exception of some early form of DECnet which got confused
	by this.

	Novell Netware used this frame type by default until the mid 90's, and since NetWare was then
	widespread, and IP was not, at some point time most of the worlds Ethernet traffic ran over 
	raw 802.3 carrying IPX. Since NetWare 4.10, NetWare defaults to IEEE802.2 with LLC 
	(NetWare Frame Type Ethernet_802.2) when using IPX.

IEEE 802.2 LLC
	Some protocols, such as those designed for the OSI stack, operate directly on top of IEEE
	802.2 LLC encapsulation, which provides both connection-oriented and connectionless network
	services.

	IEEE 802.2 LLC encapsulation is not in widespread use on common networks currently, with
	the exception of large corporate NetWare installations that have not yet migrated to NetWare
	over IP. In the past, many corporate networks used IEEE 802.2 to support transparent 
	translating bridges between Ethernet and Token Ring or FDDI networks.

	There exists an Internet Standard for encapsulating IPv4 traffic in IEEE802.2 LLC Service
	Access Point(SAP)/SubNetwork Access Protocol(SNAP) frames. It is almost never implemented
	on Ethernet, although it is used on FDDI, Token Ring, IEEE802.11(With the exception of the
    	5.9GHz band, where it uses ethertype) and other IEEE802 LANs. IPv6 can also be transmitted
	over Ethernet using IEEE 802.2 LLC Service Access Points(SAPs)/SubNetwork Access Protocol(SNAP),
	but, again, that is almost never used.

IEEE 802.2 SNAP
	By examining the 802.2 LLC header, its is possible to determine whether it is followed by a
	SNAP header. The LLC header includes two 8-bit address fields, called service access points
	(SAPs) in OSI terminology; when both source and destination SAP are set to the value 0xAA,
	the LLC header is followed by a SNAP header. The SNAP header allows Ethertype values to be
	used with all IEEE 802 protocols, as well as supporting private protocol ID spaces.

	In IEEE 802.3x-1997, the IEEE Ethernet standard was changed to explicitly allow the use
	of the 16 bit field after the MAC addresses to be used as a length field or a type field.

	The AppleTalk v2 protocol suite on Ethernet ("EtherTalk") uses IEEE 802.2 LLC + SNAP 
	encapsulation.

Maximum Throughput
	We may calculate the protocol overhead for ethernet as a percentage(packet size including IPG)

		Protocol overhead = (Packet size - Payload size) / Packet size

	We may calculate the protocol efficiency for Ethernet
		
		Protocol efficiency = Payload size / Packet size

	Maximum effeciency achieved with largest allowed payload size is 

		1500 / 1538 = 97.53%

	For untagged frames, since the packet size is maximum 1500 octet payload + 8 octet preamble +
	14 octet header + 4 octet trailer + minimum interpacket gap (IPG) corresponding to 12 octets
	= 1538 octets. The maximum efficiency is:

		1500 / 1542 = 97.28%

	when 802.1Q VLAN tagging is used.

	The throughput may be calculated from efficiency

	Throughput = Efficiency * Net bit rate,

	where the physical layer net bit rate(the wire bit rate) depends on the Ethernet physical layer
	standard, and may be 10 Mbit/s, 100 Mbit/s, 1 Gbit/s or 10 Gbits. Maximum throughput for
	100BASE-TX Ethernet is consequently 97.53Mbit/s without 802.1Q, and 97.28 Mbit/s with 802.1Q.

	Channel utilization is a concept often confused with protocol efficiency. it considers only
	the use of the channel, disregarding the nature of the data transmitted - either payload or 
	overhead. At the physical layer, the link channel and equipment do not know the difference
	between data and control frames. We may calculate the channel utilization

		Channel Utilization = Time Spent Transmitting Data / Total Time

	The total time considers the round-trip time along the channel, the processing time in the
	hosts and tthe time transmitting data and acknowledgements. The time spent transmitting
	data includes data and acknowledgements 


WHAT WE LEARNED: 
	The ethernet packet consists of frames that hold data in the form of binary that is
	converted to decimal for Lsb-first and hexadecimal for buses. I find it important to note
	that when doing serious security research checking different ethernet standards is a good
	call. The connection between a PHY and a MAC uses a bus from the MII family. Octets are bytes.
	These frames consists of the destination/source, type/length, and paylod along with a 32-bit
	Cyclic Redundancy Cycle (CRC). The length can vary with Ethernet type used and there is an 
	InterPacket Gap(IPG) with a minimum 12 octets. There are several frame types with
	Ethernet II being the most common. 
	 
source:wikipedia
---
PHY: Physical layer of Ethernet, converts bits to voltage signals

Physical Layer
	In the seven-layer OSI model of computer networking, the physical or layer 1 is the first
	and lowest layer: the layer most closely associated with the physical connection between
	devices. The physical layer provides an electrical, mechanical, and procedural interface to
	the transmission medium. The shapes and properties of the electrical connectors, the 
	frequencies to transmit on, the line code to use and similar low-level parameters, are
	specified by the physical layer.

	At the electrical layer, the physical layer is commonly implemented in a dedicated PHY chip or
	in Electronic Design Automation (EDA), by a design block. In mobile computing, the MIPI 
	Alliance*-PHY family of interconnect protocols are widely used.

	* The MIPI Alliance is a global business alliance that develops technical specifications for
	  the mobile ecosystems, particularly smartphones. *
Role
	The physical layer defines the means of transmitting a stream of raw data bits over a physical
	data link connecting network nodes. The bitstream may be grouped into code words or symbols
	and converted into a physical signal that is transmitted over a transmission medium.

	The physical layer consists of the electronic circuit transmission technologies of a network.
	It is a fundamental layer underlying the higher level functions in a network, and can be
	implemented through a great number of different hardware technologies with widely varying
	characteristics.

	Within the semantics of the OSI model, the physical layer translated logical communications
	requests from the data link layer into hardware- specific operations to cause transmission
	or reception of electronic(or other) signals. The physical layer supports higher layers
	responsible for generation of logical data packets.

Physical Signaling Sublayer
	In a network using Open Systems Interconnection (OSI) architecture, the physical signaling
	sublayer is the portion of the physical layer that:
		- Interfaces with the data link layer's Medium Access Control (MAC) sublayer
		- Performs symbol encoding, transmission, reception and decoding
		- Performs galvanic isolation*

	* Galvanic isolation - A technique where function sections of an electrical system are
	 seperated to prevent direct current flow while still allowing signals and power transfer*  

Relation to the Internet Protocol Suite
	The internet protocol suite, as defined in RFC 1122 and RFC 1123*, is a high-level networking
	description used for the Internet and similar networks. It does not define a layer that deals
	exclusively with hardware level specifications and interfaces, as this model does not concern
	itself directly with physical interfaces.
	
	* RFC - Request for comments, a formal documentation system for specifications and details 
	  by the Internet Engineering Taskforce (IETF)*
Services
	The major functions and services performed by the physical layer are: The physical layer 
	performs bit-by-bit or symbol-by-symbol data delivery over a physical transmission medium.
	It provides a standardized interface to the transmission medium, including a mechanical
	specification of electrical connectors and cables, for example maximum cable length, an
	electrical specification of transmission line signal level and impedance. The physical layer
	is responsible for electromagnetic comptatiblity including electromagnetic spectrum frequency
	allocation and specification of signal strength, analog bandwidth, etc. The transmission
	medium may be electrical or optical over optical fiber or a wireless communication link such
	as free-space optical communication or radio.

	Line coding is used to convert data into a pattern of electrical fluctuations which may be
	modulated onto a carrier wave or infared light. The flow of data is managed with bit 
	synchronization in synchronous serial communication or start-stop signaling and flow control
	in asynchronous serial communication. Sharing of the transmission medium among multiple 
	network participants can be handled by simple circuit switching or multiplexing. More complex
	medium access control protocols for sharing the transmission medium may use carrier sense 
	and collision detection such as Ethernet's Carrier-sense multiple access with collision
	detection (CSMA/CD).

	To optimize reliability and efficiency, signal processing techniques such as equalization,
	training sequences and pulse shaping may be used. Error correction codes and techniques
	including forward error correction may be applied to further reliability.

	Other topics associated with the physical layer include: bit rate; point-to-point, mulitpoint
	or point-to-multipoint line configuration; physical network topology, for example bus, ring,
	mesh, or star network; serial or parallel communication; simplex, half duplex or full duplex
	transmission mode; and autonegotiation.

PHY
	A PHY, an abbreviation for physical layer, is an electronic circuit, usually implemented
	as an integrated circuit, required to implement physical layer in functions of the OSI model 
	in a network interface controller.

	A PHY connects a linklayer device (often called MAC) to a physical medium such as an optical
	fiber or copper cable. A PHY device typically includes both Physical Coding Sublayer(PCS) and
	Physical Medium Dependant (PMD) layer functionality.

	PHY may also be used as suffix to form a short name referencing a specific physical layer of
	protocol, for example M-PHY

	Modular transceivers for the fiber-optic communication ( like the SFP family ) complement
	a PHY chip and form the PMD sublayer. 

Ethernet physical transceiver
	The Ethernet PHY is a component that operates at the physical layer of the OSI network model.
	It implements the physical layer portion of the Ethernet. Its purpose is to provide analog
	signal physical access to the link. It is usually interfaced with a Media-Independant
	Interface (MII) to a MAC chip in a microcontroller or another system that takes care of
	higher layer functions.

	More specifically, the Ethernet PHY is a chip that implements the hardware send and receive
	function of Ethernet frames; it interfaces between the analog domain of Ethernet's line 
	modulation and the digital domain of the link-layer packet signaling. The PHY usually does
	not handle MAC addressing, as that is the link layer's job. Similarily Wake-On-Lan and Boot
	ROM functionality is implemented in the Network Interface Card (NIC), which may have PHY, 
	MAC, and other functionality integrated into one chip or as seperate chips.

	Common Ethernet interfaces include fiber or two to four copper pairs for data communication.
	However, there now exists a new interface, called Single Pair Ethernet (SPE), which is able
	to utilize a single pair of copper wires while still communicating at the intended speeds.
	Texas Instruments DP838XX family are examples of PHY that use SPE.

Other Applications
	Wireless LAN or WI-FI: The PHY portion consists of the RF, mixed signal and analog portions,
	that are often called transceivers, and the digital baseband portion that use Digital Signal
	Processor(DSP) and communication algorithm processing, including channel codes. It is common 
	that these PHY portions are integrated with the medium access control (MAC) layer in 
	system-on-a-chip (SOC) implementations. Similiar wireless applications include 
	3G/4G/5G/, WiMAX and UWB.
	
	Universal Serial Bus (USB): A PHY chip is integrated into msot USB controllers in hosts or
	embedded systems and provide the bridge between digital and modulated parts of the interface

	IrDA: The Infared Data Association's (IrDA) specification includes an IrPHY specification
	for the physical layer of teh data transport.

	Serial ATA (SATA): Serial ATA controllers use PHY



WHAT WE LEARNED:

A PHY is a digital interface block for getting high speed data on or off the chip. It is called a 
PHY in reference to the OSI Networking model, which calls the lowest level the “physical layer” or 
“PHY”. You would potentially be working on things like PLLs, custom logic, high-speed ADCs and stuff
like that. The data rates are typically in 10s of Gb/s or more and often are in the most advanced 
nodes because PHYs need to go on almost every chip like SoCs and so on.

- TheAnalogKoala

source:wikipedia / redditors
---
MAC: Media Access Control - Handles addressing and channel access
     Medium Access Control - Still handles addressing and channel access. They are the same.

     Do not confuse with Mandatory access control. They are not the same. Although you should
     definitely study this.

	In IEEE802 LAN/MAN standards, the MAC is the layer that controls the hardware responsible
	for interaction with the wired(electrical or optical) or wireless transmission medium. The MAC
	sublayer and the Logical Link Control (LLC) sublayer together make up the data link layer.
	The LLC provides flow control and multiplexing for the logical link (Ethertype, VLAN tag etc,)
	while the MAC provides flow control and multiplexing for the transmission medium.

	These two sublayers together correspond to layer 2 of the OSI model. For compatibility reasons,
	LLC is optional for implementations of IEEE 802.3 (the frames are then 'raw'), but compulsory
	for implementations of other IEEE 802 physical layer standards. Within the heirarchy of the
	OSI model and IEEE 802 standards, the MAC sublayer provides a control abstraction of the
	physical layer such that the complexities of physical link control are invisible to the LLC
	and the upper layers of the network stack. This any LLC sublayer ( and higher layers) may be
	used with any MAC. In turn, the medium access control block is formally connected to the PHY
	via a media-independant interface. Although the MAC block is today typically integrated with
	the PHY within the same device package, historically any MAC could be used with any PHY,
	independent of the transmission medium.

	When sending data to another device on the network, the MAC sublayer encapsulates higher-level
	frames into frames appropriate for the transmission medium(i.e The MAC adds a syncword
	preamble and also padding if neccassary), adds a frame check sequence to identify transmission
	errors, and then forwards the data to the physical layer as soon as the appropriate channel
	access method permits it. For topologies with a collision domain (bus, ring, mesh, point-to-
	multipoint topologies), controlling when data is sent and when to wait is neccassary to
	avoid collisions. Additionally, the MAC is also responsible for compensating for collisions
	by intiating retransmission if a jam signal is detected. When receiving data from the
	physical layer, the MAC block ensures data integrity by verifying the sender's framecheck
	sequences, and strips off the sender's preamble and padding before passing the data up to
	the higher layers.

Functions performed in the MAC sublayer
	According to IEEE Std 802-2001 section 6.2.3 "MAC sublayer". the primary functions performed
	by the MAC layer are:
		- Frame delimiting and recognition
		- Addressing of destination stations ( both as individual stations and as groups of
		  stations)
		- Conveyance of source-station addressing information
		- Transparent data transfer of LLC PDUs, or of equivalent information in the Ethernet
		  sublayer
		- Protection against errors, generally by means of generating and checking frame check
		  sequences
		- Control of access to the physical transmission medium

	In the case of Ethernet, the functions required of a MAC are:
		- Receive/transmit normal frames
		- Half-duplex retransmission and backoff functions
		- Append/check FCS
		- Interframe gap enforcement
		- Discard malformed frames
		- Prepend(tx)/remove(rx)preamble, SFD, and padding
		- Half-duplex compatibility: append(tx)/remove(rx) MAC address

Addressing Mechanism
	The local network addresses used in IEEE 802 networks and FDDI networks are called MAC 
	addresses; they are based on the addressing scheme that was used in early Ethernet 
	implementations. A MAC address is intended as a unique serial number/ MAC addresses are
	typically assigned to network interface hardware at the time of manufacture. The most
	significant part of the address identifies the manufacturer, who assigns the remainder of
	the address, thus providing a potentially unique address. This makes it possible for frames 
	to be delivered on a network link that interconnects hosts by some combination of repeaters,
	hubs, bridges and switches, but not by network layer routers. Thus, for example, when an IP
	packet reaches its destination (sub)network, the destination IP ( a layer 3 or network layer
	concept) is resolved with the Address Resolution Protocol(ARP) for IPv4, or by Neighbour 
	Discovery Protocol (IPv6) into the MAC  address ( a layer 2 concept) of the destination host.

	Examples of physical networks are Ethernet networks and WIFI networks, both of which are IEEE
	802 48-bit MAC addresses.

	A MAC layer is not required in full duplex point-to-point communication, but address fields 
	are included in some point-to-point protocols for compatibility reasons.

Channel Access Control Mechanism
	The channel access control mechanisms provided by the MAC layer are also known as multiple
	access method. This makes it possible for sever stations connected to the same physical 
	medium to share it. Examples of shared media are bus networks, ring networks, hub networks,
	wireless networks and half duplex point-to-point links. The multiple access method may detect
	or avoid data packet collisions if a packet mode contention based channel access method is 
	used, or reserve resources to establish a logical channel if a circuit-switched or
	channelization-based channel access method is used. The channel access control mechanism relies
	on a physical layer multiplex scheme.

	The most widespread multiple access method is the contention-based CSMA/CD used in Ethernet
	networks. This mechanism is only utilized within a network collision domain, for example, 
	an Ethernet bus network or a hub-based star topology network. An Ethernet network may be
	divided into several collision domains, interconnected by bridges and switches. A multiple
	method is no required in a switched full-duplex network, such as today's switched Ethernet
	networks, but is often available in the equipment for compatability reasons.

Channel Access Control Mechanism for control mechanism
	Use of directional antennas and millimeter-wave communication in a wireless personal area 
	network increase the probabily of concurrent scheduling of non-interfering transmissions
	in a localized area, which results in an immense increase in network throughput. However,
	the optimum scheduling of concurrent transmissions is an NP-hard problem
	
Cellular Networks
	Cullular networks such as GSM, UMTS, or LTE networks also use a MAC layer. The MAC protocol
	in cellular network is designed to maximize the utilization of the expensive licensed 
	spectrum. The air interface of a cellular network is at layers 1 and 2 of the OSI model; at
	layer 2, it is divided into multiple protocol layers. In UMTS and LTE, those protocols are
	the Packet Data Convergence Protocol (PDCP), The Radio Link Control (RLC), and the MAC
	protocol. The base station has absolute control over the air interface and schedules the
	downlink access as well as the uplink access of all devices. The MAC protocol is specified
	by 3GPP in TS 25.321 for UMTS, TS 36.321 for LTE and TS 38.321 for 5G

WHAT WE LEARNED:
	MAC can stand for medium access control OR media access control. MAC sublayer encapsulates
	higher-level frames appropriate for transmission medium. 


source:wikipedia
-------------------------------------------------------------

C. Ethernet To Network Interface Controller (NIC)

General knowledge:
The NIC receives electrical signals and parses the bitstream into structured frames
Sends these frames to the kernel driver of the OS (software level (STACK) starts here)

---
NIC: Your systems network adapter
	A newtork interface controller or NIC, also known as a network interface card, network
	adapter, or LAN adapter is a hardware component that connects a computer to a computer
	network. It serves as the interface between a computer's internal system and the physical
	network medium, such as Ethernet cables, fiber optic cables, or wireless signals.

	The primary function of a NIC is to prepare, send, and control the flow of data on the
	network. It operates at both the physical layer(Layer 1) and the data link layer (layer 2)
	of the OSI model, handling the transmission of data frames between devices on the same
	network.


Comonents of a NIC
	A type NIC consists of serveral key components such as:
	- Physical Connection Interface: This is the port where the network cable connects (such as
	  RJ45 for Ethernet) or where wireless antennas are attached.
	- MAC Address: Each NIC has a unique MAC address burned into its ROM, which serves as its
	  identifier on the network.
	- Bus Interface: This allows the NIC to communicate with the computer's Central Processing
	  Unit(CPU) and memory via interfaces like PCI, PCIe, USB etc
	- Controller chipset: The main processor on the NIC that manages network communication
	  protocols.
	- Buffer Memory: Temporary storage for data packets being sent or received.
	- Boot ROM Socket: Some NICs include this for network booting capabilities 

Types of Network Interface Controllers
	Based on connection metherod:
		- Internal NICs: These are expansion cards that plug directly into a computer's
		  motherboard. Modern internal NICs typically use PCI Express slots, though older
		  systems might use PCI or even ISA slots. Many motherboads now come with integrated
		  NICs, eliminating the need for seperate expansion cards

		- External NICS: These connect to a computer through external ports like USB,
		  Thunderbolt, or other peripheral connections. They're useful for adding network
		  capabilities to devices with limited interna; expansion options or for adding
		  network interfaces.

	Based on Network Technology
		- Ethernet NICs: The most common type supporting various speeds of:
			Fast Ethernet (100 Mbps)
			Gigabit Ethernet (1 Gbps) 
			10 Gigabit Ethernet (10 Gbps)
			40/100 Gigabit Ethernet (for high-performance computing and data centers)

		- Wireless NICs: These enable connection  to wireless netowrks using standards
		  such as WIFI4 (802.11n), WIFI5 (802.11ac), and WIFI6/6E (802.11ax)

		- Fiber Channel NICs: Used primarily in Storage Area Networks (SANs) for high-speed
		  data transfer between servers and storage systems

		- InfiniBand NICs: Designed for high-performance computing applications with
		  extremely low latency and high throughput

How NICs Work: The Technical Process
	The operation of a NIC involves several steps in the data transmission process:
	
	Sending Data:
		1. The operating system passes data to the NIC driver.
		2. The driver formats the data into packers with the appropriate headers.
		3. The NIC controller adds the necessary frame information, including MAC address.
		4. The NIC converts the digital signals into the approppraite electrical, optical,
		   or radio signals.
		5. These signals are transmitted over the network medium.


	Receiving Data:
		1. The NIC receives signals from the network medium
		2. It converts these signals back into digital data
		3. The NIC ecamines the MAC address to determine if the packet is intended for this
		   device
		4. If it is, the NIC generates an interrupt to notify the CPU that data has arrived.
		5. The data is transferred to the system memory where the operating system can
		   process it.

Programming interfaces for NICs
	If you're a programmer, you'll interact with NICs through various interfaces. Here are
	the most common ones:
	


	Socket API
	
	The socket API is the most widely used interface for network programming.  It provides a set
	of functions for creating network connections, sending and receiving data, and managing
	network resources. Here's a simple example example of a TCP socket client in C:



	#include <stdio.h>
	#include <stdlib.h>
	#include <string.h>
	#include <unistd.h>
	#include <arpa/inet.h>
	#include <sys/socket.h>

	int main() {
		int sock = socket(AF_INET, SOCK_STREAM, 0);
		struct sockaddr_in server_addr;

		server_addr.sin_family = AF_INET;
		server_addr.sin_port = htons(8080)
		server_addr.sin_addr.s_addr = inet_addr("127.0.0.1")

		connect(sock, (struct sockaddr*)&server_addr, sizeof(server_addr));

		char message[] = "Hello from client";
		send(sock, message, strlen(message), 0);

		char message[] = "Hello from client";
		send(sock, message, strlen(message), 0);

		char buffer[1024];
		recv(sock, buffer, 1024, 0);

		printf("Server response: %s/n", buffer);
		close(sock);

		return 0;
	}


Raw Sockets
	Raw sockets provide direct access to the underlying network protocols, allowing you to
	create custom packet headers and implement your own protocols. This is useful for network
	monitoring, packet sniffing and developing security tools.

	#include <stdio.h>
	#include <stdlib.h>
	#include <string.h>
	#include <unistd.h>
	#include <sys/socket.h>
	#include <netinet/ip.h>
	#include <netinet/in.h>


	int main() {
		int sock = socket(AF_INET, SOCK_RAW, IPPROTO_RAW);
		if (sock == -1) {
			perror("Failed to create raw socket");
			exit(1);
		}

		// Now you can create custom packets

		close(sock);
		return 0;
	}

Packet Capture Libraries
	Libraries like libpcap ( for Unix-like systems) and WinPcap/Npcap (for Windows) provide high-
	level interfaces for capturing and analyzing network packets. These are commonly use in
	network analysis tools and intrusion detection systems.


	#include <pcap.h>
	#include <stdio.h>

	void packet_handler(u_char *user_data, const struct pcap_pkthdr *pkthdr, const u_char *packet){
	     printf("Packet captured. Length: %d\n", pkthdr->len);
	}

	int main() {
		char errbuf[PCAP_ERRBUF_SIZE];
		pcap_t *handle;

		handle = pcap_open_live("eth0", BUFSIZ, 1, 1000, errbuf);
		if (handle == NULL) {
			fprintf(stderr, "Could not open device: %s\n", errbuf);
			return 1;
		}

		pcap_loop(handle, 10, packet_handler, NULL); // Capture 10 packets

		pcap_close(handle);
		return 0;
	}


Network Driver Interface Specification (NDIS)
	NDIS is Microsoft's API for network interface cards. It provides standard interface between
	the transport and network layers and data link layer. Developers can create NDIS drivers or
	use the NDIS API to interact with network adapters at a low level.

Data Plane Development Kit (DPDK)
	DPDK is a set of librarie and drivers for fast packet processing. it bypasses the kernel
	networking stack to achieve high-performance packet processing, making it ideal for
	applications like network function virtualization.

Advanced NIC features for programmers
	Modern NICs offer several advanced features that programmers can leverage for improved
	performance and functionality:
	
	1. TCP Offloading Engine (TOE)
	TOE moves TCP/IP processing from the CPU to the NIC, reducing CPU load and improving network
	performance. This is particularly useful for high throughput applications.

	2. Receive Side Scaling (RSS)
	RSS distributes network receive processing across multiple CPU cores, which is essential for
	scaling network applications on multi-core systems

		// Example of enabling RSS in Linux using ethtool
		system("ethtool -L eth0 combined 4"); // set combine channels for RSS

	3. SR-IOV ( Single Root I/O Virtualiztion)
	SR-IOV allows a single physical NIC to appear as multiple virtual NICS, enabling efficient
	network virtualiztion. This is particularly important in cloud computing and virtualized
	environments.

	4. RDMA (Remote Direct Memory Access)
	RDMA enables direct memory access from the memory of one computer to another without
	involving the operating system, resulting in high-throughput, low-latency networking

	#include <rdma/rdma_cma.h>

	// Simplified RDMA connection setup
	struct rdma_cm_id *id;
	struct rdma_event_channel *channel;

	channel = rdma_create_event_channel();
	rdma_create_id(channel, &id, NULL, RDMA_PS_TCP);
	// Continue with connection setup and data transfer

Practical Applications and Programming Considerations
	Network Programming Best Practices:
	1. Use Non-blocking I/O: For high-performance applications. non-blocking I/O or asynchronous
	I/O can significantly improve throughput

	2. Buffer Management: Efficient buffer management is crucial for minimizing memory usage
	and maximizing throughput.

	3. Error Handling: Network programming requires robust error handling to deal with 
	disconnections, timeouts, and other network issues.

	4. Protocol Selection: Choose the appropriate protocol (TCP, UDP, SCTP) based on your
	application's requirements for reliability, ordering, and speed.


	Common Use Cases:
	1. High Performance Network Applications: For applications requiring thoughput consider:
		- Using DPDK for kernel bypass 
		- Leveraging hardweare offloading features
		- Implementing zero-copy techniques to reduce memory operations

	2. Network Monitoring and Analysis: For building tools like packet sniffers or analazyers:
		- Use libpcap/WinPcap for cross-platform packet capture
		- Implement efficient filtering to process only relevant packets
		- Consider hardware acceleration pattern matching
		
	3. Virtualized Environment: When working with virtual machines or containers:
		- Utilize SR-IOV for direct hardware access
		- Consider DPDK virtual device interfaces for high performance
		- Implement proper isolation between virtual interfaces.


Troubleshooting NIC issues in CODE
	When developing network applications, you may encounter various NIC-related issues, Here are
	some common problems and soltuions:
	

	1. Performance Bottlenecks
	Symptoms: High CPU usage, lowthroughput, high latency

	Solutions:
	- Use profiling tools to identify bottle necks
	- Enable hardware offloading features
	- Consider using kernel bypass frameworks like DPDK
	- Optimize buffer sizes and memory allocation


	2. Packet Loss
	Symptoms: Missing Data, retransmissions, degraded performance

	Solutions:
	- Increase socket buffer sizes
	- Check for NIC buffer overflows using monitoring tools
	- Implement flow control mechanism
	- Consider using a more reliable protocol if applicable

		// Example of increasing socket buffer size in C
	int buffer_size = 262144; //256kb
	setsockopt(sock, SOL_SOCKET, SO_RCVBUF, &buffer_size, sizeof(buffer_size));

	3. Compatibility Issues
	Symptoms: Connection failures, unexpected behavior across different systems

	Solutions:
	- Use Standard, well supported API's
	- Implement feature detection and fallback mechanisms
	- Test on multiple platforms and with different NIC models

Future Trends in NIC Technology
	As a programmer, its important to stay informed about emerging NIC technologies that may
	impact your network applications

	1. SmartNICs and DPUs
	Smart Network Interface Cards and Data Processing Units include programmable processors that
	can run custom code directly on the NIC. This enables offloading complex networking taks,
	security functions, and even application-specific processing to the network card.

	2. 400GbE and Beyond
	With the continual increase in network and bandwidth requirements, 400 GigaBit Ethernet is
	becoming more common and 800GbE and 1.6TbE standards are in development. These ultra-high-
	speed interfaces will require new programming approaches to fully utilize their capabilities

	3. Software Defined Networking(SDN) Integration
	NICs are increasingly incorporating features to support SDN, allowing for more flexible 
	network configuration and management through programmable interfaces.
WHAT WE LEARNED:
A lot.
Network Interface Controllers are fundamental components in modern computing systems, serving as the
bridge between computers and networks. For programmers, understanding how NICs work and how to 
effectively program them is essential for developing efficient network applications.

From basic socket programming to advanced techniques like RDMA and kernel bypass, the way you 
interact with NICs can significantly impact your application’s performance and capabilities. By 
leveraging the appropriate programming interfaces and taking advantage of modern NIC features, you 
can develop network applications that achieve optimal performance, reliability, and scalability.

As network technologies continue to evolve, staying informed about the latest NIC developments will 
be crucial for programmers looking to build next-generation network applications. Whether you’re 
developing high-performance trading systems, real-time multimedia applications, or cloud 
infrastructure, a deep understanding of NICs will be an invaluable asset in your programming toolkit.

source:algoacademy
---
Interrupts/DMA: Communicates to the main memory without CPU

Direct Memory Access
	Direct Memory Access (DMA) is a technique used in computers and the other electronic devices
	to allow peripherals (like hard drives, network cards, and sound cards) to communicate 
	directly with the main memory (RAM) without involving the CPU. This process speeds up data
	transfer and frees up the CPU to perform other tasks, improving overall system performance.

	- The peripheral device sends a request to the DMA controller to initiate a data transfer.
	- The DMA controller takes control of the system's memory bus and accesses memory directly,
	either reading data from it or writing data to it.
	- After the transfer is complete, the DMA controller signals to the CPU that the task is
	finished, and the CPU can continue other tasks

Working of DMA Transfer							

     Address Bus							|-------------------------|
<-----------------------------------------------------------------------|Address Bus Buffer       |
      Data Bus     	________________________________	|       |-------------------------|
<----------------------|   Data Bus Buffer		|------>|
		       |________________________________|	|     |---------------------------|
    DMA Select     |__________________________________________| |<--->|    Address Register       |
------------------>|					      | |     |---------------------------|
  Register Select  |		Control Logic		      | |
------------------>|					      | |     |---------------------------|
       Read        | 					      | |<--->|    Word Count Register    |
<----------------->|					      | |     |---------------------------|
       Write       |					      | |
<----------------->|					      | |     |---------------------------|
    Bus Request    |					      | |<--->|     Control Register      |
<------------------|					      | |     |---------------------------|
		   |                                          | |
     Bus Grant	   |                                          | |
------------------>|					      | | Internal Bus
		   | 					      | |
		   |					      |
    Interrupt      |					      |		DMA Request
<------------------|					      |<----------------------------------
		   |					      |
		   |					      |---------------------------------->
		   *------------------------------------------*     DMA Acknowledge


DMA Controller Components
1. Controller Logic: The Control Logic is the central component that manages the overall DMA
operation. It processes control signals and directs data transfers between the peripherals and memory.
It receives commands from other components and determines how and when data should be moved.

2. DMA Select and DMA Request: DMA Select is used by the DMA controller to select the appropriate data
transfer request. DMA Request is initiated by a peripheral device when it needs to preform a data
transfer. The request tells the DMA controller that the device is ready to either read or write data.

3. DMA Acknowledge: The DMA Acknowledge signal is send back from the control logic to the peripheral
device to confirm that the DMA operation has been initiated and the device can proceed with the data
transfer.

4. Bus Request and Bus Grant: Bus Request is generated by the DMA controler when it needs access 
to the system's bus for data transfer. The Bus Grant Signal is sent from the CPU or the system's bus
controller to give the DMA controller persission to use the bus for transferring data.

5. Address Bus and Data Bus: The Address Bus and Data Bus are used to transfer data and memory
addresses between the DMA controller, memory, and peripherals. The Data Bus Buffer temporarily holds
data being transferred, while the Address Bus Buffer holds memory addresses.

6. Registers
	Address Register: This stores the memory address where data will be written or read from.

	Word Count Register: This keeps track of the number of words or units of data that need to
	be transferred.

	Control Register: This contains control information, including the direction of data
	transfer(read / write), and other control signals necessary to manage the DMA operation

7. Internal Bus: The Internal Bus connects all the components inside the DMA controller, allowing
them to communicate and pass data efficiently

8. Interrupt: The interrupt signal is used to inform the CPU once the DMA operation is complete. After
the data has been transferred, the DMA controller sends an interrupt to notify the CPU, so the CPU can
resume processing or handle other tasks.

Working:
	- The DMA controller facilitates the transfer of data between memory and peripherals without
	involving the CPU for each individual data operation, as mentioned in the article.

	- The DMA Select and DMA Request initiate the process when a peripheral wants to transfer
	data, similar to how DMA allows peripherals to operate independently if the CPU.

	- Address Bus and Data Bus handle the flow of data and memory addresses during the transfer,
	improving system effficiency by bypassing the CPU.

	- The Registers ( Address Register, Word Count Register, Control Register) store the
	neccessary information to control the transfer, as described in the article, where DMA
	controls the movement of data between the device and memory

	- The Interrupt is triggered once the transfer is completed, similiar to how the CPU is 
	notified in the article that DMA operations have been finished.

Types of DMA
	There are several types of DMA, each with its own way of transferring data.

	Burst Mode DMA: In this mode, the DMA controller takes control of the memory bus and
	transfers a block of data in one go. The CPU is temporarily locked out of memory acces while
	the DMA controller completes the data transfer

	Cycle Stealing DMA: In this mode the DMA controller transfers one data item at a time but 
	allows allows  the CPU to access memory between each transfer. This allows the CPU and DMA
	controller to share the memory bus and more collaboratively.

	Block Mode DMA: The DMA controller transfers a block of data without interruption, but it
	uses a more organized approach than burst mode, allowing for more efficient transfers.
	The CPU is locked out of memory access during the transfer.

	Demand Mode DMA: In this mode, the DMA controller transfers data only when the CPU is not 
	using the memory bus, essentially waiting for an idle time to perform the transfer
WHAT WE LEARNED:
	We learned that DMA stands for Direct Access Memory. Its a technique used to communicate
        directly with the RAM without involving the CPU. It has multiple components that work
        together to make this possible. There are also several types of DMA, each with its own
        way of transferring data.

source:geeksforgeeks
---
MTU(Maxiumum Transmission Unit): Usually 1500 bytes per ethernet frame.
This is the default max packet size(without fragmentation)

What is MTU?
	The Maximum Transmission Unit (MTU) us the largest amount of data that can be transmitted
	in a single packet on a network. It is a parameter that is determined by the underlying 
	network technology, and can be configured on network devices such as routers and switches.

	The MTU is important because it determines the maximum size of data that can be transmitted
	across a network without fragmentation. If a packet is larger than the MTU, it is fragmented
	into smaller packets to be transmitted across the network, and then reassembled at the
	receiver. Fragmentation can result in additional processing overhead and increased network
	traffic, which can impact performance.

	The MTU is typically specified in bytes, and can vary depending on the network technology
	being used. For example, Ethernet networks typically have an MTU of 1500 bytues, while
	some WAN technologies may have an MTU of 9000 bytes or higher.

	In order to avoid fragmentation and ensure optimal network performance, it is important to
	ensure that the MTU is configured correctly on all devices on the network. This can be 
	done through various means, such as adjusting the MTU on the network interfaces or using
	technologies such as Path MTU Discovery to dynamically adjust the MTU based on the
	characteristics of the network path.

	The larger MTU result in more data transmission during a single connection, therefore, reduces
	the overhead. On the other hand, the smaller MTU can be transferred faster, because of tis size,
	thus reducing delay on the network. Therefore the size of the MTU should be adjusted to
	optimize both requirements.

	The default size of the maximum transmission unit is 1500 B.

Working of MTU
	Let us suppose the Internet's Transmission Control Protocol (TCP) specified the size of
	MTU = 750 B, that is the maximum protocol data unit size that can be delivered from source
	to destination. In such a scenario, the following case may arise.
		- If the system sends packets larger than the size of the MTU, that is the
		 packet size = 750B in this case, then the system packet will be fragmented
		 to smaller packets such that their size doesn't exceed the largest packet size.
		The process of division of a large data packet into smaller chunks of data such that
		none of these chunks exceed the maximum frame size is called Fragmentation. These
		are later reassembled at the final client destination.

		 ___________				________     ________	  ________
		|   1600 B  | -----------------------> |  750 B |   |  750 B |   |  100 B |
		*-----------*			       *--------*   *--------*   *--------*
							    |____________|___________| 
								     Fragments


	If the system sends packets within MTU size, they are transmitted as a single frame in the
	network connection. Howeber, packets much small than MTU may increase delay and cause 
	network inefficiency. Reassembling packets in such case is not requried.


		________				________
	       |  600 B | -------------------------->  |  600 B |
		*------*				*------*
							    |
						      Single Fragment


Applications
	The maximum transmission unit has the following applications:
		- MTU is used over the internet, primarily the TCP to determine the optimal packet
		size.
		- It is associated with Ethernet Protocol, and is referred as Protocol Data Unit(PDU)

Issues in MTU:
	There are several issues associated with the MTU in computer networking.
	
	1. Fragmentation: If a packet is larger than the MTU of a particular network segment, it 
	needs to be fragmented into smaller packets to be transmitted across the network. This
	fragmentation can cause additional overhead and latency, which can impact network
	performance and reliability.
	
	2. Path MTU Discovery: In some cases, the MTU can vary along the path between the sender and
	receiver. The can be a result in packets being dropped or delayed due to fragmentation, as
	the sender may not be aware of the correct MTU to use for each segment of the network path.
	Path MTU Discovery is a technique used to dynamically adjust the MTU based on the
	characteristics of the network path.

	3. Jumbo Frames: Some network technologies support larger MTUs, known as jumbo frames.
	While this can improve network performance in certain scenarios, it can also introduce
	compatibility issues with devices that do not support jumbo frames.

	4. Security: In some cases, attack can exploit MTU-related vulnerabilities to launch denial-
	of-service (DoS) attacks or to bypass network security measures such as firewalls.

Reference:
	Here are some references related to MTU in computer networking
	1. RFC 791: Internet Protocol(IP) - This document defines the IP protocol,including the MTU,
	which is used to determine the maximum size of IP packets.

	2. RFC 1191: Path MTU discovery - This document describes a technique used to dynamically
	discover the MTU along a network path, in order to avoid fragmentation and improve network
	performance

	3. Cisco Networking AcademyL CCNA Routing and Switching - Scaling networks - This course
	covers the MTU and other key concepts related to network scaling and performance.

	4. Juniper Networks: Understanding MTU and TCP MSS - This article provides a detailed
	overview of the MTU and its relationship to the TCP Maximum Segement Size (MSS)

	5. Network World: The trouble with jumbo frames - This article discusses the potential
	benefits and drawbacks of using jumbo frames, which support larger MTUs than standard
	Ethernet frames 
WHAT WE LEARNED:
	Stands for Maximum Transmission Unit. Parameter for largest amoung of data in a packet.
	Typically in bytes. Fragmentation can result in additional procsessing. Some WANS have 9000
	MTU while Ethernet is around 1500.
source:geeksforgeeks (I actually really like the way this site writes their info)
--------------------------------------------------------------

E. Kernel / Network Stack(Layer 3-4)

General Knowledge:
The NIC hands the data to the kernels TCP/IP stack
The stack processes:
	IP packets - Layer 3: Routing info, TTL, fragmentation
	TCP/UDP segments - Layer 4: portnumbers, sequencing, reliability

*** This guide is going to branch into Unix systems, perhaps one day I will GitGud and do a windows
	network stack ***


Network Stack
	One of the largest kernel subsystem is a network stack. It is called a stack because it 
	consists from multiple protocols where each of them works on top of the more primitive
	protocol. That hierarchy is defined by different models such as OSI model or TCP/IP stack.
	When user data is passed through a network, it is incapsulated into packets of that protocol:
	when data is passed to a protocol driver it puts some service data to the packet header, as
	well as the tail so the operating system on receiver host can recognize them and build the 
	original message. Each layer of the network stack has its responsibilities so they are not of
	concern to higher-layer protocols. For example, IP allows sending of datagrams through multiple
	routers and networks, can reassembly packets but doesnt gaurntee reliability when some data
	is lost -- it is implemented in TCP protocol. Both of them can only transmit raw data, encoding
	or compression is implemented on a higher layer such as HTTP

	Network subsystem (which transmits data between hosts) has a major difference over block input-
	output ( which stores data): it is bery sensitive to latency, so writing or reading data 
	cannot be deffered. Due to that, sending and receiving is usually performed in the same
	thread context.

	A network stack in Unix systems can be split into three generic layers:
	- Socket Layer which implements BSD sockets through a series of system calls.
	- Intermediate protocol drivers such as ip, udp, and tcp and packet filters.
	- Media Access Control (MAC) layer on the bottom which providing acces to NICs and NIC API
	itself. It is called GLD in Solaris.

	Network input-output can require transferring huge amounts of data, so it may be ineffective 
	to explicitly send write commands for each packet. instead of handling each packet 
	individually, NIC and tis driver maintain a shared ring buffer where dirver puts data in while
	the card used DMA mechanisms to read data and send it over the network. Ring buffers are 
	defined by two pointers: A head and a tail.
	      _____
	     |	   |__   ___
	head--    | O |-| , |--
                   ---   ---   |
			      ---
			     | W |
			      ---
			       |
			      ---
			     | O |
			      ---
			 ___ __|
	      Tail |----| R |
			 ---

Application sends packet l

              _____
             |     |__   ___
        head--    | O |-| , |--
                   ---   ---   |
                              ---
                             | W |
                              ---
                               |
                              ---
                             | O |
                              ---
                    ___  ___ __|
              Tail-| l |-| R |
                    ---   ---

NIC sends out packet O

              ____________
             |           _|_			___
        head--          | , |--		       | O | -----> I'm free!
                         ---   |		---
                              ---
                             | W |
                              ---
                               |
                              ---
                             | O |
                              ---
                   ___   ___ __|
            Tail--| l |-| R |
                   ---   ---

When a driver wants to queue packet for transmission it put it into memory area of designated ring
buffer and updates tail pointer appropriately. When NIC transfers data over a network it will
update head pointer.

Data structures are usually shared between stack layers. In Linux packets are represented by a generic
sk_buff structure
The structure keeps two pointers: head and data and includes offsets for protocol headers. Data length
is kept in len field, time stamp of packet in tstamp field, sk_buff structures form a double-linked 
list through next and prev structures. They refer network device descriptor which is represented by 
net_device structure and a socket which is represented by pair of structures: socket which holds 
generic socket data including file pointer which points to VFS node(sockets in Linux and solaris 
are managed by special filesystems) and sock which keeps more network-related data including local
address which is kept in skc_rcv_saddr and skc_num and peer address in skc_daddr and skc_dport 
correspondingly.

Note that CPU byte ordermay differ from network byte order, so you should use conversion functions to
work with addresses such as ntohs, ntohl or ntohll to convert to host byte order and htons, htonl, 
and htonll for reverse conversions. They are provided by SystemTap and Dtrace and have same behavior
as their C ancestors.
Here is a sample script for tracing message receiving in Linux 3.9:
** stap - SystemTap **
# stap -e '
	probe kernel.function("tcp_v4_rcv") {
		printf("[%4s] %11s:%-5d -> %11s:%-5 len: %d\n",
		kernel_string($skb->dev->name),

		ip_ntop($skb->__sk_common->skc_rcv_saddr),
		$skb->sk->__sk_common->skc_num,

		$skb->len);
	}'

For earlier versions that use inet_sock:
# stap -e '
    probe kernel.function("tcp_v4_do_rcv") {
        printf("%11s:%-5d -> %11s:%-5d len: %d\n",
                ip_ntop(@cast($sk, "inet_sock")->daddr), 
                ntohs(@cast($sk, "inet_sock")->dport),
                    
                ip_ntop(@cast($sk, "inet_sock")->saddr), 
                ntohs(@cast($sk, "inet_sock")->sport),
                    
                $skb->len);
    }'

WHAT WE LEARNED:
That we are now analog players in a digital world.
The network stack consists of three generic layers for UNIX. Socket Layer, Intermediate protocol drivers
such as ip,udp and tcp and packet filters, as well as a MAC layer.
source:some dtrace doc?
https://myaut.github.io/dtrace-stap-book/kernel/net.html


To be totally honest if you are interested you should take a look here as well
https://linux-kernel-labs.github.io/refs/heads/master/labs/networking.html


---

Internet Protocal (IP)
	The third network-level protocol is the Internet Protocol(IP), which provides unreliable,
	connectionless packet delivery for the Internet.

	IP is connectionless because it treats each packet of information independently. It is
	unreliable because it does not gaurntee delivery, nmeaning, it does not require acknowledgments
	from the sending host, the receiving host, or the intermediate hosts.

	IP provides the interface to the network interface level protocols. The physical connections
	of a network transfer inormation in a frame with a header and data. The header contains the
	source address and the destination address. IP uses an Internet datagram that contains 
	information similiar to the physical frame. The datagram also has a header containing IP
	addresses of both source and destination of the data.

	IP defines the format of all the data sent over the internet.

	Bits
	0         4        8                    16      19
	*---------*--------*--------------------*-------*-------------------------*
	| Version | Length |   Type of Service  |           Total Length	  |
	*------------------*--------------------+-------+-------------------------*
	|		Identification		| Flags |    Fragment Offset      |
	*------------------.--------------------+-------+-------------------------*
	|   Time To Live   |        Protocol    |        Header Checksum          |
	*------------------*--------------------*---------------------------------*
	| 			     Source Address				  |
	*-------------------------------------------------------------------------*
	|			    Destination Address				  |
	*-------------------------------------------------------------------------*
	|			       Options					  |
	*-------------------------------------------------------------------------*
	|				Data					  |
	*-------------------------------------------------------------------------*
Honestly I thought my little chart was cool but:
https://www.rfc-editor.org/rfc/rfc791.html

This illustration shows the first 32 bits of a typical IP packet header. The table below lists the
various entities.
	Version:
		Specifies the version of IP used. The current version of IP protocol is 4.

	Length:
		Specifies the datagram header length, measured in 32-bit words.

	Type of Service:
		Contains five subfields that specify the type of precedence, delay, throughput, and
		reliability desired for that packet. (The Internet does not gaurnteee this request)
		The default settings for these five subfields are routing prescedence, normal delay,
		normal throughput, and normal reliabilty. This field is not generally use by the
		internet at this time. This implementation of IP complies with the requirements of the
		IP specification, RFC 791, Internet Protocol.

	Total Length:
		Specifies the length of the datagram including both the header and the data measured
		in ocets. Packet fragmentation at gateways, with reassembly at destinations, is 
		provided. The total length of the IP packet can be configured on an interface-by-
		interface basis with tha ifconfig command, or the System Management Interface Tool
		(SMIT) fast path, smit chinet. Use SMIT to set the values permanently in the 
		configuration database; use the ifcommand to set or change the values in the running
		system.

	Identification:
		Contains a unique integer that identifies the datagram.

	Flags:
		Controls datagram fragmentation, along with the Identification field. The fragment
		flags specify whether the datagram can be fragmented and whether the current fragment
		is the last one.

	Fragment Offset:
		Specifies the offset of this fragment in the original datagram measured in units of
		8 octets.

	Time To Live:
		Specifies how long the datagram can remain on the Internet. This keeps misrouted
		datagrams from remaining on the Internet indefinitely. The default time to live is
		255 seconds

	Protocol:
		Specifies the high-level protocol type.

	Header Checksum:
		Indicates a number computed to ensure the integrity of header values.

	Source Address:
		Specifies the Internet address of the sending host

	Destination Address: 
		Specifies the Internet address of the receiving host

	Options:
		Provides network testing and debugging. This field is not required for every datagram.
		
		End of Option List:
		Indicates the end of the option list. It is used at the end of the final option, not 
		at the end of each option individually. This option should be used only if the end of
		the options would not otherwise coincide with the end of the IP header. End of Option
		List is used if options exceed the length of datagram.

		No operation:
		Provides alignment between other options; for example, to align the beginning of a
		subsequent option on a 32 bit boundary.

		Loose Source and Record Route:
		Provides a means for the source of an Internet datagram to supply routing information
		used by the gateways in forwarding the datagram to a destination and in recording the
		route information. This is a loose source route: the gateway or host IP is allowed
		to use any route of any number of other intermediate gateways in order to reach the 
		next address in the route.

		Strict Source and Record Route:
		Provides a means for the source of an Internet datagram to supply routing information
		used by the gateways in forwarding the datagram to a destination and in recording the
		route information. This is a strict source route: In order to reach the next gateway or
		host specified in the route, the gateway or hsot IP must send the datagram directly 
		to the next address in the source route and only to the directly connected network that
		is indicated in the next address.

		Record Route:
		Provides a means to record the route of an Internet datagram

		Stream Identifier:
		Provides a way for a stream identifier to be carried through networks that do not
		support the stream concept.

		Internet Timestamp
		Provides a record of the time stamps through the route.

Outgoing packets automatically have IP header preficed to them. Incoming packets have their IP
header removed before being sent to the higher-levl protocols. The IP protocol provides for the
universal addressing of hosts in the Internet network.

		
WHAT WE LEARNED
	IP is connectionless.
source:IBM
---
I would highly recommend studying IP protocols or atleast look at a chart. For this we will move to
TCP/IP model.

TCP/IP model
	The TCP/IP model (Transmission Control Protocol/Internet Protocol) is a four layer networking
	framework that enables reliable communication between devices over interconnected networks. It
	provides a standardized set or protocols for transmitting data across interconnected networks,
	ensuring efficient and error-free delivery. Each layer has specific functions that help manage
	different aspects of network communication, making it essential for understanding and working
	with modern networks.

	TCP/IP was designed and developed by the Department of Defense (DoD) in the 1970's and is
	based on standard protocols. The TCP/IP model is a conise version of the OSI model. It contains
	four layers, unlike the seven layers in the OSI model. 

Role of TCP/IP
	TCP/IP enables interoperability between diverse systems over various network types(e.g copper,
	fiber, wireless). It ensures seamless communication across LANs, WANs, and the internet. 
	Without TCP/IP, large scale global networking would not be possible.

	The main condition of this process is to make data reliable and accurate so that the receiver
	will receive the same information which is sent by the send. To ensure that, each message
	reaches its final destination accurately, the TCP/IP model divides its data into packets
	and combines them at the other end, which helps in maintaining the accuracy of the data while
	transferring from one end to another end.

	The diagrammatic comparison of the TCP/IP model is as follows:

		OSI	            TCP/IP
	---------------------.-----------------.
	| Application Layer  |                 |
	*--------------------+                 |
	| Presentation Layer |   Application   |
	*--------------------+     Layer       |
	| Session Layer	     |	       	       |
	*--------------------+-----------------*
	| Transport Layer    | Transport Layer |
	*--------------------+-----------------*
	| Network Layer      | Network Layer   |
	*--------------------+-----------------*
	| Data Link Layer    |    Network      |
	*--------------------+    Access       |
	| Physical Layer     |    Layer        |
	*--------------------*-----------------*

	1. Application Layer
	The Application Layer is the closest to the end user and is where applications and user 
	interfaces reside. It serves as the brige between user programs and the lower layers 
	responsible for data transmission.
	- Function: Provide services and interfaces for end-user applications to access resources.
	- Key responsibilities:
		- Supports application protocols like HTTP, FTP, SMTP, DNS etc.
		- Enables communication between software applications across networks.
		- Handles data formatting, encryption, and session management.

	2. Transport Layer
	This layer ensures data is delivered reliably and in the correct order between devices. The 
	two main protocols in this layer are TCP ( Transmission Control Protocol) and UDP ( User
	Datagram Protocol).
	- Function: Ensures reliable or unriable delivery of data between hosts.
	- Key responsibilities:
		- TCP: Provides reliable, connection-oriented delivery with error checking,
		retransmission, and flow control.
		- UDP: Provides fast, connectionless transmission without gaurantees.
		- Manages flow control and segmentation/reassembly of data

	3. Internet Layer
	It handles the routing of data packets across networks. It uses the internet protocol to
	assign unique IP addresses to devices and decide the most efficient path for data to reach its
	destination.
	- Function: Determines the best path for data to travel across networks
	- Key responsibilities:
		- IP: Provides addressing and routing
		- Handles packet forwarding, fragmentation, and logical addressing(IP addresses).
		- Involves protocols like IP, ICMP(for diagnostics), and ARP(for address resolution).

	4. Network Access Layer
	This layer is the lowest layer in the model and responsible for the physical connection between
	devices within the same network segment.
	- Function: Manages the physical transmission of data over the network hardware
	- Key responsibilities:
		- Handles how data is physically sent over cables, WIFI, etc
		- Manages MAC addressing, framing, and error detection at the physical link.
		- Includes Ethernet, WIFI, and other data link technologies.

Working of TCP/IP Model
	When Sending Data(From Sender to Receiver)
	
	Application Layer
		- A user data through an application (e.g opening a website via a browser).
		- The application prepares data for transmission (e.g using HTTP, FTP, SMTP).

	Transport Layer(TCP/UDP)
		- TCP breaks data into small segments, adds a header ( with sequence numbers, source/
		destination ports).
		- Ensures reliable delivery (TCP) or fast, connectionless delivery(UDP).

	Internet Layer(IP)
		- Adds IP addresses to each packet (source and destination).
		- Determines the route the packet should take to reach the destination.

	Link Layer(Network Access Layer)
		- Converts packets into frames, adds MAC (physical) addresses.
		- Sends data as binary bits (0s and 1s) over the physical medium (e.g Ethernet, WIFI).



		     |  Sending Host  |	     |	Receiving Host |
		      ----------------        -----------------
			     |
-----------------.    -----------------         ----------------
Application Layer|   | Creates Request |       |Performs Request|
-----------------*    -----------------         ----------------
			     |			       ^
----------------.     -----------------         ----------------
Transport Layer |    | TCP/UDP Segment |       | TCP/UDP Segment|
----------------*     -----------------         ----------------
			     |                         ^   
---------------.      -----------------         ---------------
Internet Layer |     |   IP Datagram   |       |  IP Datagram  |
---------------*      -----------------         ---------------
			     |		              ^
---------------.      -----------------		---------------
Data Link Layer|     |	    Frame       |      |     Frame     |
---------------*      -----------------         --------------- 
			     |				|
			      ------------ >> ----------

When Reveiving Data ( At the destination )
	Link Layer:
	- Receives bits and reconstructs frames.
	- Passes frames up to the Internet layer.

	Internet Layer:
	- Reads the IP address to confirm its the correct recipient.
	- Removes the IP header and sends the data to the Transport layer

	Transport Layer:
	- Reassembles TCP segments in the correct order
	- Verifies data integrity using acknowledgements and checksums

	Application Layer
	- The data is delivered to the appropriate applciation (e.g browser displays web page)

Why TCP/IP is used over the OSI model
	Simpler Structure:
	TCP/IP has only 4 layers, compared to 7 in OSI, making it easier to implement and understand
	in real systems

	Protocol Driven Design:
	TCP/IP was designed based on working protocols, while OSI is more of theroretical framework

	Flexibility:
	TCP/IP is open, free to use, and not controlled by any single organization, helping it gain
	universal acceptance.

	Actual use VS conceptual Use:
	The OSI model is great for education and design principles, but TCP/IP is the one actually
	used in real-world networking.

Advantages of TCP/IP model
	Interoperability:
	The TCP/IP model allows different types of computers and networks to communicate with each
	other, promoting compatibility and cooperation among diverse systems.

	Scalability:
	TCP/IP is highly scalable, making it suitable for both small and large networks,
	from local area networks (LANs) to wide area networks (WANs) like the internet.

	Standardization: 
	It is based on open standards and protocols, ensuring that different devices and software can
	work together without compatibility issues.

	Flixibility:
	The model supports various routing protocols, data types, and communication methods, making
	it adabptable to different working needs.

	Reliability: 
	TCP/IP includes error-checking and retransmission features that ensure reliable data transfer,
	even over long distances and through various network conditions.

Disadvantages of TCP/IP model
	Security Conerns: 
	TCP/IP was not originally designed with security in mind. While there are
	now many security protocols available ( SSL/TLS ), they have been added on top of the basic
	TCP/IP model, which can lead to vulnerabilities.

	Inefficiency for Small Networks:
	For very small networks, the overhead and complexity of the TCP/IP model may be unneccessary
	and inefficient compared to simpler networking protocols.

	Limited by Address Space:
	Although IPv6 addresses this issue, the older IPv4 system has a limited address space, which
	can lead to issues with address exhuastion in larger networks.

	Data Overhead:
	TCP the transport protocol, includes a significant amount of overhead to ensure a reliable
	transmission. This can reduce efficiency, especially for small data packets or in networks
	where speed is crucial

WHAT WE LEARNED:

source:geeksforgeeks
---
Ports: Logical end points 
Im not typing out every port *** ADD A MARKDOWN CHART FOR PORTS or something ***
------------------------------------------------------------

E. Application Layer & User Space(Layer 5-7)

General Knowledge:
Packet is handed to the user-space application - browser or curl
If a response is need (SYN -> SYN-ACK), the system forms a return packet and
transmits it back through the stack.
-----------------------------------------------------------


Questions:

How does the machine know what to do:

The machine is "open" and just listening (and vulnerable if not secured properly) 

Minimal Trigger
A single ethernet frame (e.g ARPm ICMP, TCP SYN) can be enough to trigger logic
	- A modern server could recieve 10Gbps+ of such frames

Packet Size
A minimum Ethernet Frame = 64 bytes
A SYN scan sends tiny TCP packets with just a flag
Even one maliscious TCP packet can cause a denial of service if a vulnerable
service mishandles it


------

Can I hook up my own machine to coax?

Technically yes.

A software-defined radio (SDR) or signal generator can be used to emit signals
BUT 
to interface meaningfully you need:
	- DOCSIS chipsets or emulation
	- Proper RF modulation
	- Authentication to ISP

Illegality:
- Injecting signals into coax == FCC violation(North America)
- Jamming frequencies even accidental = federal offense
- Easily traced, especially in urban areas.

--------------

Why?

The Pioneer Mindset

Early engineers had no blueprints
They physically tested waveforms, timing, and noise thresholds
Designed hand crafted modems before protocols existed
Built signal processors and designed chips by hand
















''')

