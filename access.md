# {{ page.title }}

In addition to the Ethernet and Wi-Fi connections we typically use to
connect to the Internet at home, at work, at school, and in many
public spaces, most of us connect to the Internet over an *access* or
*broadband* service that we buy from our local ISP. This section
describes two such technologies: *Passive Optical Networks* (PON),
commonly referred to as fiber-to-the-home or fiber-to-the-premises,
and *Celluar Networks* that connect our mobile devices. In both cases,
the networks are multi-access (like Ethernet or Wi-Fi), but as we will
see, their approach to mediating access is quite different.

> DSL is the legacy, copper-based counterpart to PON. DSL links are
> also terminated in Telco Central Offices, but we do not describe
> this technology since it is being phased out.

To set a little more context, ISPs (often a Telco or Cable company)
typically operate a national backbone, and connected to the periphery
of that backbone are hundreds or thousands of edge sites, each of
which serves the local city or neighborhood. These edge sites are
commonly called *Central Offices* in the Telco world and *Head Ends*
in the cable world, and despite their names implying "centralized" and
"root of the hierarchy" these are the points at which the ISP directly
connects to their customers; the ISP-side of the last mile. PON and
Cellular access networks are anchored in these facilities.

## Passive Optical Network

PON is the technology most commonly used to deliver fiber-based
broadband to homes and businesses. PON adopts a point-to-multipoint
design, which means the network is structured as a tree, with a single
point starting in the ISP's network and then fanning out to reach
multiple homes (up to 1024). PON gets its name from the fact that the
splitters are passive: they forward optical signls downstream and
upstream without actively storing-and-forwarding frames. Framing
then happens at the source in the ISP's premises, called an *Optical
Line Terminal* (OLT), and at the end-points in individual homes,
called an *Optical Network Unit* (ONU).

For this reason, PON has to implement some form of multi-access
protocol. The approach it adopts is easily summarized as
follows. First, upstream and downstream traffic are transmitted on two
diffrent optical wavelengths, so they are completely independent of
each other. Downstream traffic starts at the OLT and the signal is
propogated down every link in the PON. As a consequence, every frame
reaches every ONU. This device then looks at a unique identifier in
the individual frames sent over the wavelength, and either keeps the
frame (if the identifier is for it) or drops it (if not). Encryption
is used to keep ONUs from eavesdropping on their neighbors' traffic.

Upstream traffic is then time-division multiplexed on the upstream
wavelength, with each ONU periodically getting a turn to
transmit. Because the OLTs are distributed over a fairly wide area and
at different distances from the OLT, it is not practical for the them
to transmit based on synchronized clocks, as in SONET. Instead, the
ONT transmits *grants* to the individual ONUs, giving them a time
interval during which they can transmit. In other words, the single
OLT is responsible for enforcing the round-robin sharing of the shared
PON. This includesthe possibility that the OLT can grant each ONU a
different share of time, effectively implementing different levels of
service.

PON is similar to Ethernet in the sense that it defines a sharing
algorithm that has evolved over time to accommodate higher and higher
bandwidths. G-PON (Gigabit-PON) is the most widely deployed today,
supporting a bandwidth of 2.25-Gbps. XGS-PON (10 Gigabit-PON) is just
now starting to be deployed.

## Celluar Network

While cellular telephone technology had its beginnings around voice 
communication, data services based on cellular standards are now
the norm, thanks in no small part to the increasing capabilities
of smartphones. Like Wi-Fi, celluar networks transmit data at certain
bandwidths in the radio spectrum. Unlike Wi-Fi, which permits anyone
to use a channel at either 2.4 or 5 GHz (all you have to do is set up a
base station), exclusive use of various frequency bands have been
auctioned off and licensed to service providers, who in turn sell
mobile services to consumers.

The frequency bands that are used for cellular networks vary around the
world, and the full allocation is complicated by the fact that ISPs
have to simultaneously support both old/legacy technologies and
new/next-generation technologies, each of which occupies a different
frequency band. The high-level summary is that the full gamet of
cellular technolgies range from 700-MHz to 1900-MHz. One interesting
footnote is that there is also an unlicensed band at 3.5-GHz set
aside in North America, called *Citizens Broadband Radio Service*
(CBRS), that anyone with a celluar radio can use. This opens the door
for setting up private cellular networks.

Like 802.11, cellular technology relies on the use of base 
stations that are part of a wired network. The geographic area served by 
a base station's antenna is called a *cell*. A base station could serve 
a single cell or use multiple directional antennas to serve multiple 
cells. Cells don't have crisp boundaries, and they overlap. Where they 
overlap, a mobile device could potentially communicate with multiple base 
stations. At any time, however, the device is in communication with,
and under the control of, just one base station. As the phone begins to
leave a cell, it moves into an area of overlap with one or more other cells.
The current base station senses the weakening signal from the phone and
gives control of the phone to whichever base station is receiving the
strongest signal from it. If the device is involved in a call or other network
session at the time, the session must be transferred to the new base
station in what is called a *handoff*. 

There have been multiple generations of protocols implementing the
cellular network, colloquially known as 1G, 2G, 3G, and so on. The first
two generations supported only voice, with 3G defining the transition
to broadband access, supporting data rates measured in hundreds of
kilobits-per-second. Today, the industry is at 4G (supporting data rates
typically measured in the few megabits-per-second) and in the process
of transitioning to 5G (with the promise of a tenfold increase in data
rates).

The generational designation is part marketing and part technical, with
different implementations actually being deployed. LTE, which stands for
*Long-Term Evolution*, is the implementation most people associate with
4G. LTE is actually a family of implementations that include both
time-division and frequency-division multiplexing. We briefly describe
the frequency-division approach here.

To start, LTE uses multi-carrier modulation, which is to say, multiple
independent signals are transmitted within the frequency band
allocated to an ISP. On top of these multiple carrier signals, LTE
implements an approach that evolved out of an earlier 3G technique
called CDMA (Code Division Multiple Access).

CDMA uses a form of spread spectrum to multiplex the traffic from 
multiple devices into a common wireless channel. Each transmitter uses a 
pseudorandom chipping code at a frequency that is high relative to the 
data rate and sends the exclusive OR of the data with the chipping code. 
Each transmitter's code follows a sequence that is known to the intended 
receiver—for example, a base station in a cellular network assigns a 
unique code sequence to each mobile device with which it is currently 
associated. When a large number of devices broadcast their signals in 
the same cell and frequency band, the sum of all the transmissions looks 
like random noise. However, a receiver who knows the code being used by 
a given transmitter can extract that transmitter's data from the 
apparent noise. 

Compared to other multiplexing techniques, CDMA has some good
properties for bursty data. There is no hard limit on how many users
can share a piece of spectrum—you just need to make sure they all
have unique chipping codes. The bit error rate does however go up with
increasing numbers of concurrent transmitters. This makes it very well
suited for applications where many users exist but at any given instant
many of them are not transmitting—which pretty well describes many
data applications such as web surfing. And, in practical systems when
it is hard to achieve very tight synchronization among all the mobile 
handsets, CDMA achieves better spectral efficiency (i.e., it gets closer 
to the theoretical limits of the Shannon-Hartley theorem) than other 
multiplexing schemes like TDMA.







