# H4cker Message
**Points:** 100
**Category:** Forensics
**Author:** [skarthik](https://github.com/karthiksenthil)
> A hacker is trying to communicate to his fellow hacker regarding a malicious event ! We have managed to sniff his network activities from NSA.
> Help us find out what this message is.


## Write up
This is probably the easiest and most solved question of ECTF'14 :)

The given file is PCAP(Packet Capture) file. On opening the same using Wireshark we notice a lot of noise in the packets. However on filtering by protocol we notice some packets being transmitted using the FTP protocol. Since the question points out that something was exchanged on this network, we can follow this stream and find that a super_secret_message.png was transferred. Next by filtering for FTP-DATA protocol packets we find one packet where a 3972B file was exchanged. On following the same we get the PNG file dump in raw. Copying that to an empty file, we obtain the flag image.  

> flag{ThIs_Is_sO_1337}

## External Write ups
http://shankaraman.wordpress.com/2014/10/19/ectf-2014-forensics-100-h4cker-message/
