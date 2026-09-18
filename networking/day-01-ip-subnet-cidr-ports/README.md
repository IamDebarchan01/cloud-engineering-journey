Today I focused on the basics of IPv4 networking:

IP addresses
Private/Public IP
Subnets
CIDR notation
Network address
Broadcast address
Usable host addresses
Ports

1. IP Address — First Practical Check

An IPv4 address identifies a device/interface on a network.

Example:

192.168.1.10

An IPv4 address contains 4 octets:

192 . 168 . 1 . 10

Each octet can contain:

0 - 255
My machine

I checked the IP configuration of my Windows machine.

ipconfig

I looked for:

IPv4 Address
Subnet Mask
Default Gateway

Example:

IPv4 Address : 192.168.1.10
Subnet Mask  : 255.255.255.0
What I understood

My computer doesn't just have an IP address.

The IP works together with a subnet mask to determine which devices belong to the same network.

2. Private IP Address

Private IP addresses are used inside private networks such as:

Home Wi-Fi
Office networks
Cloud VPCs

Common private IPv4 ranges:

10.0.0.0      → 10.255.255.255

172.16.0.0    → 172.31.255.255

192.168.0.0   → 192.168.255.255

For example:

Laptop      → 192.168.1.10
Phone       → 192.168.1.11
Printer     → 192.168.1.20

These can all exist inside the same private network.

3. What Is a Subnet?

A subnet is a smaller network created from a larger IP network.

Instead of thinking about subnetting as complicated mathematics, I currently think about it as:

Large network
     ↓
Split into smaller networks
     ↓
Subnets

Example:

192.168.1.0/24

can contain many devices.

If the network is divided further, we can create smaller networks such as:

192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26

The important thing I learned today is that the CIDR number determines how much of the IP belongs to the network portion.

4. CIDR — /24

CIDR tells us how many bits are used for the network portion.

Example:

192.168.1.0/24

/24 means:

24 bits → Network
8 bits  → Hosts

IPv4 has 32 bits total:

32 - 24 = 8

Therefore:

2^8 = 256

There are 256 total addresses.

But not all 256 can normally be assigned to devices.

For a traditional IPv4 subnet:

256 total
- 1 network address
- 1 broadcast address
---------------------
254 usable host addresses

So:

192.168.1.0/24

Network:   192.168.1.0
Usable:    192.168.1.1 - 192.168.1.254
Broadcast: 192.168.1.255
5. Practical CIDR Experiment — /30

I practiced with a much smaller subnet:

172.168.3.0/30

A /30 leaves:

32 - 30 = 2 host bits

Therefore:

2^2 = 4 total addresses

The addresses are:

172.168.3.0
172.168.3.1
172.168.3.2
172.168.3.3

Now identify their purpose:

172.168.3.0 → Network address
172.168.3.1 → Usable host
172.168.3.2 → Usable host
172.168.3.3 → Broadcast address

Therefore:

Total addresses = 4
Usable hosts    = 2
My mental shortcut

For a /30:

/30
 ↓
2 host bits
 ↓
2² = 4 addresses
 ↓
2 usable hosts
6. Network Address vs Host Address vs Broadcast

I practiced identifying the three different types of addresses.

For:

192.168.1.0/24
Network
192.168.1.0

This represents the entire network.

Host
192.168.1.1
192.168.1.2
192.168.1.3
...
192.168.1.254

These can normally be assigned to devices.

Broadcast
192.168.1.255

This represents the broadcast address for that subnet.

Simple picture
192.168.1.0/24

┌─────────────────────────────────┐
│           NETWORK               │
│                                 │
│  .0      .1 ............ .254   │
│   ↑       ↑                ↑    │
│ Network  Hosts           Hosts  │
│                                 │
│              .255               │
│                ↑                │
│            Broadcast            │
└─────────────────────────────────┘
7. Practical Subnet Calculation

I practiced this example manually:

192.168.10.0/26

A /26 leaves:

32 - 26 = 6 host bits

Therefore:

2^6 = 64 total addresses

Usable:

64 - 2 = 62 hosts

The first subnet is:

Network:    192.168.10.0
Hosts:      192.168.10.1 - 192.168.10.62
Broadcast:  192.168.10.63

The next subnet starts at:

192.168.10.64

So the next range is:

Network:    192.168.10.64
Hosts:      192.168.10.65 - 192.168.10.126
Broadcast:  192.168.10.127
What helped me understand it

Instead of memorizing random numbers, I looked at the block size:

/26 → 64 addresses per subnet

0
64
128
192

Therefore:

0 - 63
64 - 127
128 - 191
192 - 255
8. Ports — Practical Test

An IP address identifies a machine/interface.

A port identifies a service/application endpoint on that machine.

Example:

192.168.1.10:443

Here:

192.168.1.10 → IP address
443           → Port

So I can think:

IP   = Which machine?
Port = Which service?

Some common ports I learned:

Port	Common use
22	SSH
80	HTTP
443	HTTPS
53	DNS
3389	RDP

I am not going deeper into these protocols yet. For now I am focusing on understanding what a port represents.

9. Practical Port Test

On Windows PowerShell:

Test-NetConnection google.com -Port 443

This tests connectivity to TCP port 443.

The important part for today's learning:

ComputerName : google.com
RemotePort   : 443
TcpTestSucceeded : True

The port is part of the destination:

destination IP/domain
        +
      port

For example:

192.168.1.10:443

is different from:

192.168.1.10:80

because the port identifies a different service endpoint.

10. Small Real-World Example

Imagine my home network:

Router
192.168.1.1
     │
     ├── Laptop
     │   192.168.1.10
     │
     ├── Phone
     │   192.168.1.11
     │
     └── TV
         192.168.1.12

The subnet could be:

192.168.1.0/24

So:

Network:    192.168.1.0
Usable:     192.168.1.1 - 192.168.1.254
Broadcast:  192.168.1.255

If a service is running on my laptop using port 8080, I can conceptually address it as:

192.168.1.10:8080

This helped me connect the concepts:

IP
 ↓
Identifies the device/interface

Subnet
 ↓
Defines the network range

CIDR
 ↓
Defines how much of the IP is network vs host

Port
 ↓
Identifies the service/application endpoint
11. My Practice Questions

I solved these without immediately looking at the answer.

Question 1
192.168.1.0/24

How many total addresses?

2^(32-24)
= 2^8
= 256

Usable hosts:

254
Question 2
10.0.0.0/30

Host bits:

32 - 30 = 2

Total:

2² = 4

Usable:

2
Question 3

For:

192.168.5.0/26

Total addresses:

2^(32-26)
= 2^6
= 64

Range:

Network:    192.168.5.0
Hosts:      192.168.5.1 - 192.168.5.62
Broadcast:  192.168.5.63
Question 4

What does this mean?

192.168.1.25:443

My answer:

192.168.1.25 → IP address
443           → Port
12. Commands I Practiced Today
Windows
ipconfig

View my machine's network configuration.

Test-NetConnection google.com -Port 443

Test whether TCP port 443 is reachable.

Linux

I will repeat the same concepts on Linux as I progress through the Linux/networking labs.

13. What I Can Explain Now

After today's practice, I should be able to explain:

What is an IPv4 address?
What is a private IP?
What is a subnet?
What does /24 mean?
What does /30 mean?
How many addresses are in a subnet?
What is a network address?
What is a broadcast address?
What is a usable host address?
What does an IP:Port combination mean?
Key Takeaway

The biggest thing I learned today is that networking becomes easier when I stop looking at IP addresses as random numbers.

For example:

192.168.10.0/26

can be broken down into:

192.168.10.0
       ↓
Network address

/26
 ↓
26 network bits

6 host bits
 ↓
2⁶ = 64 addresses

64 addresses
 ↓
Network + Hosts + Broadcast

And:

192.168.10.20:443

can be broken down into:

192.168.10.20
       ↓
      IP

443
 ↓
Port