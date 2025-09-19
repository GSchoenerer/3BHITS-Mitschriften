# NWTK

## Wiederholung

### Kabel/Nurmen

- Ethernet Kabel
  - Cat 5e 1GBit
  - Cat 6
  - Cat7
  - ...

- Stecker/Buchse
  - RJ45

- Norm Kabellänge
  - 100m Gesamt
  - 10m von PC zu Buchse
  - 80m im Gebäude
  - 10mm vom Patchpanel zum Switch


### Wlan
- Carrier 2.4GHz
  - 12 Kanäle
  - langsam
  - wenig user
  - geht über weite Distanzen
- Carrier 5GHz
  - ca. 50 Kanäle
  - mehr user
  - größere Datenrate
  - man kann Kanäle bündeln
- Carrier 6GHz
  - ...

### Protokolle

#### Ethernet Protokoll
- Ziel MAC: 6Byte
- Quell MAC: 6Byte
- VLan Tag: 4Byte
- Protokoll: 2Byte (zeigt welches Protokoll in den Daten steht)
- Daten: max ca. 1500Byte (in den Daten von Ethernet kann IP oder ARP stehen)
  
#### IPv4
- Versionsfeld: 0100
- IHL(Internet Header Lengh)
  - zeigt auf den beginn der Daten
  - min Wert 5
- Type of Service
  - nicht mehr in verwendung
- Total lengh: 16Bit (0-65535d) 64kByte
- ID
- Dlags(sind zum fragmentieren notwendig)
  - More Fragment
  - Last Fragment
  - Fragment Offset
- TTL(Time To Live): 8Bit
- Protokoll: Zeigt welches Protokoll in den Daten von IP steht ICMP, TCP, UDP, ...
- Header Checksum
- Src IP: 4Byte
- Quell IP: 4Byte

**Bsp.:**
Es sollen 4kByte Daten Verschickt werden (nur 1500 Byte in Ethernet möglich) also 3 Fragmente benötigt (A, B, C)

A)
- Total Lengh: 4000
- ID: 42 (zufällige zahl um sicher zu sein in kleinem Bereich)
- Flags: More Fragment
- Fragmetn Offset: 0

B)
- Total Lengh: 4000
- ID: 42
- Flags: More Fragment
- Fragment Offset: 1496/8 = 187 (daten müssen ganzahlig durch 8 Teilbar sein)

C)
- Total Lengh: 4000
- ID: 42
- Flags: Last Fragment
- Fragment Offset: 187\*2 = 374 | (1496\*2) / 8 = 374

#### ICMP(Internet Control Message Protokoll)

- Type
- Code
- Checksum

**Bsp.:**
Ping:
  - echo reply:
    - Type: 0
    - Code: 0
  - echo request:
    - Type: 8
    - Code: 0
Destination unreacheble:
  - Type: 3
  - Code: 1(Ziel Host nicht erreichbar)

##### Ping Bsp.

    ROUTER
    |── eth0: 20.255.255.254/8 (MAC: 0-0-0-0-0-2)
    │   |── PC2: 20.0.0.1/8 (MAC: 3-4-5-6-7-8)
    │
    |── eth1: 10.255.255.254/8 (MAC: 0-0-0-0-0-1)
        |── SWITCH
            |── PC1: 10.0.0.1/8 (MAC: 1-2-3-4-5-6)
            |── PC: 10.0.0.2/8
            |── PC: 10.0.0.3/8

1\) **PC1** führt ping aus
- ping 20.0.0.1
- Protokoll: ARP request
- Destination IP: /
- Source IP: /
- Destination MAC: FF:FF:FF:FF:FF:FF
- Source MAC: 1-2-3-4-5-6

2\) **Router** schickt ARP reply
- Destination IP: /
- Source IP: /
- Destination MAC: 1-2-3-4-5-6
- Source MAC: 0-0-0-0-0-1

3\) 
- Protokoll: ICMP echo request (Type: 8 Code: 0)
- Destination IP: 20.0.0.1
- Source IP: 10.0.0.1
- Destination MAC: 0-0-0-0-0-1
- Source MAC: 1-2-3-4-5-6

4\) **Im Router** ARP request
- Destination IP: /
- Source IP: /
- Destination MAC: FF:FF:FF:FF:FF:FF
- Source MAC: 0-0-0-0-0-2

5\) **PC2** ARP reply
- Destination IP: /
- Source IP: /
- Destination MAC: 0-0-0-0-0-2
- Source MAC: 3-4-5-6-7-8



**To Be Continued**