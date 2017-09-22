# pyshark

Welcome to the pyshark wiki!

***Pyshark Tutorial:***

This tutorial is an extension of https://github.com/KimiNewt/pyshark

Install pyshark from the above link.
I am going to explain on how to parse through the captured packets using pyshark.

On your command line type “python” and follow the below instructions.

```
>>> import pyshark
>>> cap = pyshark.FileCapture('Desktop/test.pcap')
>>> print (cap[0])
Packet (Length: 216)
Layer ETH:
	Destination: 01:00:5e:7f:ff:fa
	Address: 01:00:5e:7f:ff:fa
	.... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
	.... ...1 .... .... .... .... = IG bit: Group address (multicast/broadcast)
	Source: dc:a9:04:8e:b7:0d
	Type: IPv4 (0x0800)
	Address: dc:a9:04:8e:b7:0d
	.... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
	.... ...0 .... .... .... .... = IG bit: Individual address (unicast)
Layer IP:
	0100 .... = Version: 4
	.... 0101 = Header Length: 20 bytes (5)
	Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
	0000 00.. = Differentiated Services Codepoint: Default (0)
	.... ..00 = Explicit Congestion Notification: Not ECN-Capable Transport (0)
	Total Length: 202
	Identification: 0x8440 (33856)
	Flags: 0x00
	0... .... = Reserved bit: Not set
	.0.. .... = Don't fragment: Not set
	..0. .... = More fragments: Not set
	Fragment offset: 0
	Time to live: 1
	Protocol: UDP (17)
	Header checksum: 0x2edc [validation disabled]
	Header checksum status: Unverified
	Source: 10.201.11.68
	Destination: 239.255.255.250
	Source GeoIP: Unknown
	Destination GeoIP: Unknown
Layer UDP:
	Source Port: 51933
	Destination Port: 1900
	Length: 182
	Checksum: 0x670d [unverified]
	Checksum Status: Unverified
	Stream index: 0
Layer SSDP:
	M-SEARCH * HTTP/1.1\r\n
	Expert Info (Chat/Sequence): M-SEARCH * HTTP/1.1\r\n
	M-SEARCH * HTTP/1.1\r\n
	Severity level: Chat
	Group: Sequence
	Request Method: M-SEARCH
	Request URI: *
	Request Version: HTTP/1.1
	HOST: 239.255.255.250:1900\r\n
	USER-AGENT: Google Chrome/61.0.3163.79 Mac OS X\r\n
	Full request URI: http://239.255.255.250:1900*
	HTTP request 1/1
	\r\n
	MAN: "ssdp:discover"\r\n
	MX: 1\r\n
	ST: urn:dial-multiscreen-org:service:dial:1\r\n
```
***We have 4 layers:***

1)	Eth
2)	IP
3)	UDP
4)	SSDP

How to parse through all these layers?

```
>>> from pprint import pprint
>>> packet1 = cap[0]
```

Each layer now can be accessed as ***x[‘eth’],  x[‘ip’], x[‘udp’] and x[‘ssdp’]***

Let us take a look at the IP layer.

```
>>> print (packet1['ip'])
Layer IP:
	0100 .... = Version: 4
	.... 0101 = Header Length: 20 bytes (5)
	Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
	0000 00.. = Differentiated Services Codepoint: Default (0)
	.... ..00 = Explicit Congestion Notification: Not ECN-Capable Transport (0)
	Total Length: 202
	Identification: 0x8440 (33856)
	Flags: 0x00
	0... .... = Reserved bit: Not set
	.0.. .... = Don't fragment: Not set
	..0. .... = More fragments: Not set
	Fragment offset: 0
	Time to live: 1
	Protocol: UDP (17)
	Header checksum: 0x2edc [validation disabled]
	Header checksum status: Unverified
	Source: 10.201.11.68
	Destination: 239.255.255.250
	Source GeoIP: Unknown
	Destination GeoIP: Unknown
```
Further to parse through every element on IP layer:

```
>>> IP = packet1['ip']
>>> pprint (vars(IP))
{'_all_fields': {'': 'Source GeoIP: Unknown',
                 'ip.addr': '10.201.11.68',
                 'ip.checksum': '0x00002edc',
                 'ip.checksum.status': '2',
                 'ip.dsfield': '0x00000000',
                 'ip.dsfield.dscp': '0',
                 'ip.dsfield.ecn': '0',
                 'ip.dst': '239.255.255.250',
                 'ip.dst_host': '239.255.255.250',
                 'ip.flags': '0x00000000',
                 'ip.flags.df': '0',
                 'ip.flags.mf': '0',
                 'ip.flags.rb': '0',
                 'ip.frag_offset': '0',
                 'ip.hdr_len': '20',
                 'ip.host': '10.201.11.68',
                 'ip.id': '0x00008440',
                 'ip.len': '202',
                 'ip.proto': '17',
                 'ip.src': '10.201.11.68',
                 'ip.src_host': '10.201.11.68',
                 'ip.ttl': '1',
                 'ip.version': '4'},
 '_layer_name': 'ip',
 'raw_mode': False}
```
vars() : Return the __dict__ attribute for a module, class, instance, or any other object with a __dict__ attribute.

Now we know the dictionary i.e key, value pairs.

As we can see, in the above output, it is a dictionary within a dictionary.

To print individual values of each key in the dictionary we need to do as shown below:

```
>>> print (IP._all_fields['ip.addr'])
10.201.11.68
```
You can use the same logic to print every key value pairs. 

***Hope this helps!***


