# {{ page.title }}

In addition to the Ethernet and Wi-Fi connections we typically use to
connect to the Internet at home, at work, at school, and in many
public spaces, most of us connect to the Internet over an *access* or
*broadband* service that we buy from an ISP. This section
describes two such technologies: *Passive Optical Networks* (PON),
commonly referred to as fiber-to-the-home or fiber-to-the-premises,
and *Celluar Networks* that connect our mobile devices. In both cases,
the networks are multi-access (like Ethernet and Wi-Fi), but as we will
see, their approach to mediating access is quite different.

To set a little more context, ISPs (often a Telco or Cable company)
typically operate a national backbone, and connected to the periphery
of that backbone are hundreds or thousands of edge sites, each of
which serves the local city or neighborhood. These edge sites are
commonly called *Central Offices* in the Telco world and *Head Ends*
in the cable world, but despite their names implying "centralized" and
"root of the hierarchy" these sites are the ISP-side of the last mile,
where the ISP directly connects to its customers. PON and Cellular
access networks are anchored in these facilities.

> DSL is the legacy, copper-based counterpart to PON. DSL links are 
> also terminated in Telco Central Offices, but we do not describe 
> this technology since it is being phased out. 

## Passive Optical Network

PON is the technology most commonly used to deliver fiber-based
broadband to homes and businesses. PON adopts a point-to-multipoint
design, which means the network is structured as a tree, with a single
point starting in the ISP's network and then fanning out to reach up
to 1024 homes. PON gets its name from the fact that the
splitters are passive: they forward optical signls downstream and
upstream without actively storing-and-forwarding frames. In this way,
they are the optical variant of repeaters used in the classic Ethernet.
Framing then happens at the source in the ISP's premises, called an
*Optical Line Terminal* (OLT), and at the end-points in individual
homes, called an *Optical Network Unit* (ONU).

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
wavelength, with each ONU periodically getting a turn to transmit.
Because the OLTs are distributed over a fairly wide area (measured
in kilometers) and at different distances from the OLT, it is not
practical for the them to transmit based on synchronized clocks, as in
SONET. Instead, the ONT transmits *grants* to the individual ONUs,
giving them a time interval during which they can transmit. In other
words, the single OLT is responsible for enforcing the round-robin
sharing of the shared PON. This includesthe possibility that the OLT
can grant each ONU a different share of time, effectively implementing
different levels of service.

PON is similar to Ethernet in the sense that it defines a sharing
algorithm that has evolved over time to accommodate higher and higher
bandwidths. G-PON (Gigabit-PON) is the most widely deployed today,
supporting a bandwidth of 2.25-Gbps. XGS-PON (10 Gigabit-PON) is just
now starting to be deployed.

## Celluar Network

While cellular telephone technology had its beginning in voice 
communication, data services based on cellular standards are now
the norm, thanks in no small part to the increasing capabilities
of smartphones. Like Wi-Fi, celluar networks transmit data at certain
bandwidths in the radio spectrum. Unlike Wi-Fi, which permits anyone
to use a channel at either 2.4 or 5 GHz (all you have to do is set up a
base station, as many of us do in our homes), exclusive use of various
frequency bands have been auctioned off and licensed to service
providers, who in turn sell mobile access service to their consumers.

The frequency bands that are used for cellular networks vary around
the world, and are complicated by the fact that ISPs have to
simultaneously support both old/legacy technologies and
new/next-generation technologies, each of which occupies a different
frequency band. The high-level summary is that the full gamet of
cellular technolgies range from 700-MHz to 1900-MHz. One interesting
footnote is that there is also an unlicensed band at 3.5-GHz set
aside in North America, called *Citizens Broadband Radio Service*
(CBRS), that anyone with a celluar radio can use. This opens the door
for setting up private cellular networks.

Like 802.11, cellular technology relies on the use of base stations
that are connected to a wired network. In the case of the cellular
network, the base staions are often called *Broadband Base Units*
(BBU), the mobile devices that connect to them are usually referred to
as *User Equipment* (UE), and the set of BBUs anchored at a given
Central Office and serving a particualr region is collectively called
a *Radio Access Network* (RAN).

The geographic area served by a BBU's antenna is called a *cell*.
A BBU could serve a single cell or use multiple directional antennas
to serve multiple cells. Cells don't have crisp boundaries, and they
overlap. Where they overlap, an UE could potentially communicate with
multiple BBUs. At any time, however, the UE is in communication with,
and under the control of, just one BBU. As the device begins to leave
a cell, it moves into an area of overlap with one or more other
cells. The current BBU senses the weakening signal from the phone and
gives control of the device to whichever base station is receiving the
strongest signal from it. If the device is involved in a call or other
network session at the time, the session must be transferred to the
new base station in what is called a *handoff*. Exactly about how
handoffs are managed is beyond the scope of this book.

There have been multiple generations of protocols implementing the
cellular network, colloquially known as 1G, 2G, 3G, and so on. The first
two generations supported only voice, with 3G defining the transition
to broadband access, supporting data rates measured in hundreds of
kilobits-per-second. Today, the industry is at 4G (supporting data rates
typically measured in the few megabits-per-second) and is in the
process of transitioning to 5G (with the promise of a tenfold increase
in data rates).

As of 3G, the generational designation actually corresponds to a
standard defined by the 3GPP (3rd Generation Partnership Project).
Even though its name has "3G" in it, the 3GPP continues to define the
standard for 4G and 5G, each of which corresponds to a release of the
standard. Release 15, which is now available, is considered the
demarcation point between 4G and 5G. Moreover, this standard is a
fairly high-level specification, defining a set of requirements but
leaving signifcant latitude in how the standard is implmented. For
example, LTE, which stands for *Long-Term Evolution*, is the
implementation many people associate with 4G.

We use LTE as an example to get a sense of how the RAN works.
There are three highlights. First, LTE uses a different multiplexing
scheme for downlink and upslink traffic, with a fraction of the
licensed band allocated to each. Second, for downlink traffic (from
BBU to UE), LTE uses a hybrid multiplexing scheme called OFDMA
(Orthogonal Frequency Division Multiple Access), which combines
frequence-division multiplexing (carving the downlink frequency band
into multiple sub-channels) and time-division multiplexing (allocating
one or more sub-channels to a given UE for a certain slot of time).
Giving the BBU two degrees of freedom improves its ability to squeeze
the most capacity out of limited bandwidth allocated to it. Third, for
uplink traffic (from UE to BBU), LTE uses SC-FDMA (Single Carrier,
Frequence-Division Multiple Access). The main advantage of SC-FDMA
over OFDMA is that it takes less power to transmit, which in the
uplink direction, reduces the workload on the limited batteries in our
cellphones.
