Taken from https://gist.github.com/Bluscream/f3c2adf019207c519d58fee6e41d99d1

Help me reverse engineer a cloud iptv cctv camera so i can use it locally without needing its app or its cloud;

This is the camera: https://www.amazon.de/-/en/gp/product/B0BXNM48TD/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1

This is it's app: https://play.google.com/store/apps/details?id=shix.vi.camera&hl=en&gl=US

This app works aswell: https://play.google.com/store/apps/details?id=shix.cam365.camera&hl=en&gl=US

From what i have seen in traffic logs it connects to a bunch of chinese servers using TCP;

This is info for the camera from Advanced Port Scanner:

- Hostname: rtthread
- MAC: 00:02:02:30:40:47
- Manufacturer: Amino Communications, Ltd.
- Open Ports:
  - Port 23 (TCP)
  - Port 10002 (TCP)
  - Port 10003 (TCP)

ipcam.pcapng package capture of the android app (Taken with https://emanuele-f.github.io/PCAPdroid/):
- Parsed: https://apackets.com/pcaps?pcap=647c19eceb8027d775b1cc37a85ce833.pcapng&view=charts
- Alternative: https://apackets.com/pcaps?pcap=c7cde7987caf626e7e7f0a2c105181cb.pcapng&view=charts
- Raw: https://apackets.com/api/v1/pcaps/public/download/647c19eceb8027d775b1cc37a85ce833.pcapng/ipcam.pcapng

Can i use the a9 addon with my camera aswell? And if not, can i modify/improve it to include support for mine?

Here are images from my cam/board:

Chip Markings: `BK7252UQN68 AU2406YB`
Chip Website: http://www.bekencorp.com/en/goods/detail/cid/22.html
Chip Datasheet: https://www.ccm99.com/app/discuz.php?mod=act&tid=119111&aid=4277


![IMG_20230617_225230|666x500](upload://lwYUuXC3knOekpPlKQQ8bezv7VD.jpeg)
![IMG_20230617_225216|666x500](upload://9GXbfRH9A2hMUsnW5I2yKsYkmTW.jpeg)
![IMG_20230617_225223|666x500](upload://l8p09iJP2VUt3mZNYHzagjeGp01.jpeg)
![IMG_20230617_225146|375x500](upload://ef20tFMLHrzzKxigCSdlxtC47JA.jpeg)
![IMG_20230617_225046|375x500](upload://lQTTae9R1ioOlabXbRLbY9xHzwf.jpeg)
![IMG_20230617_225136|375x500](upload://pvaG3aTLzLehhpHTBWjAv6u5zk9.jpeg)
![IMG_20230617_224817|375x500](upload://lP5vGZjGW43REhBT8lNQOEYf1xD.jpeg)
![IMG_20230617_224825|666x500](upload://n3XD4Fq7Hub48pPox0wSAgeWZ0A.jpeg)
![IMG_20230617_225038|375x500](upload://H7cMKyL4OizcWORFaAXUH0i01e.jpeg)
![Screenshot_2023-06-17_22-50-22|640x480](upload://3Im2fQie9HoZZijGkCZIB8YKrR9.jpeg)
![Screenshot_2023-06-17_22-50-12|640x480](upload://AhMz73SqDTm36SbZaxgSTGyH9Jh.png)
![Screenshot_2023-06-17_22-49-44|640x480](upload://rNdKB6BlSLo8W2YziB3Pa1npc5q.png)
![Screenshot_2023-06-17_22-49-24|640x480](upload://nmOaTGpIa9WiUcH705E7MuDK7FR.jpeg)
![Screenshot_2023-06-17_22-49-19|690x388](upload://arf51DXnTSoT6E4LeJfdYmH6PQS.jpeg)

Btw, has anyone here found a open source android / windows app that implements this p2p udp tunnel protocol?

EDIT: I now tried replaying my captured packets where i did a bunch of camera actions (pan, zoom, toggled leds) with scapy, but absolutely nothing:

```
from scapy.all import *

# list all interfaces
print(conf.iface)
print("")
packets = rdpcap("test/ipcam.pcapng")
# sendp(packets)
l = len(packets)
for i, packet in enumerate(packets):
    print(f"Sending packet {i} / {l}: {packet.summary()}")
    # print(packet)
    sendp(packet, iface="Intel(R) Ethernet Connection (11) I219-V")
```

> Sending packet 5942 / 5942: IP / UDP 10.215.173.1:19874 > 192.168.2.72:14414 / Raw