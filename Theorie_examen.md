# cnet2-leerstof-vragenlijst-21-22

> 
>
> **Algemene info over het examen:**
> \- Examen bestaat uit 2 delen: 1e deel gesloten boek, 2de deel open boek (ongeveer elk
> de helft van de beschikbare tijd)
> \- Een deel van de vragen van het gesloten boek-examen komt uit onderstaande
> vragenlijst. Bemerk dat het niet steeds de volledige of letterlijk dezelfde vraag hoeft
> te zijn die gesteld wordt. Het zal dus niet volstaan een
> studentensamenvatting/antwoordenlijst te bestuderen.
> \- Het open boek-examen zal zich voornamelijk toespitsen op theoretische oefening
> zoals we die gezien hebben in de lessen omtrent IPv4, IPv6 en Ethernet
>
> **Te kennen leerstof**
> \- Kennisclips en bijhorend slidemateriaal
> \- Respons-/oefeningencolleges en bijhorend slidemateriaal
> \- Zelftests
> \- Labo-inhoud
> \- Vermelde secties uit Kurose Ross
> \- Aanvullend pdf-hoofdstuk over IPv6
> \- Alle materiaal waarnaar verwezen wordt uit het wekelijkse leerstofdocument
>
> **Handboek**
>
> - **Hoofdstuk 2**
>   \- Sectie 2.4 DNS
>   \- Sectie 2.5 Peer-to-peer
>   \- Sectie 2.7 Sockets (zie ook Sectie 3.2)
> - **Hoofdstuk 4**
>   \- Sectie 4.3 IP
>   \- Sectie 4.4 SDN
> - **Hoofdstuk 5**
>   \- Sectie 5.1 Intro
>   \- Sectie 5.2 Algoritmes
>   \- Sectie 5.3 Intra-AS routering
>   \- Sectie 5.4 Inter-AS routering
>   \- Sectie 5.5 SDN controle-level
>   \- Sectie 5.6 ICMP
>   \- Sectie 5.7 Net mgt & SNMP
> - **Hoofdstuk 6**
>   \- Sectie 6.4 LAN met switches
> - **Hoofdstuk 7**
>   \- Sectie 7.1
>   \- Sectie 7.2
>   \- Sectie 7.3 Draadloze LANs
> - **Hoofdstuk8**
>   \- Sectie 8.9 Firewalling en IDS



## Hoofdstuk 2



1. **DNS is een hiërarchisch gedecentraliseerd systeem. Geef a.d.h.v. een voorbeeld aan hoe**
   **dit werkt**

   > DNS heeft 3 (of eigenlijk 4 als je de lokaal meetelt) soorten servers, **root** servers, **tld** (top level domain) servers en **authoritative** servers. De root servers staan vanboven aan de tree, er zijn er meer dan 400 over heel de wereld die beheerd worden door 13 organisaties. deze root servers bevatten de ip adressen van TLD servers. Die top level domain servers beheren top level domeinen zoals .com, .be, .edu, … en bevatten de ip adressen van de authoritative servers. Een authoritative server is een server van het bedrijf zelf die opgezet werd die zelf verwijst naar hun eigen webserver of bestemming. Wanneer je bv naar amazon.com surft zal eerst de root server gecontacteerd worden die verwijst door naar het .com tld, die tld verwijst dan door naar de amazon.com authorative sever die dan zelf zegt ah gebruik dat ip maar.

   ![Imgur](https://imgur.com/jvSxTWk.png)

   

2. **Een lokale DNS-server werkt meestal zowel op recursieve als iteratieve wijze. Bespreek**
   **beide werkwijzen. Leg uit welke methode gebruikt wordt voor lokale hosts.**

> **Recursief** : De vraag wordt doorgespeeld naar andere servers tot aan de server die het antwoord kan geven.



![Imgur](https://imgur.com/A1xlVHQ.png)



> **Iteratief**: De vraag wordt niet doorgespeeld naar andere servers. De server stelt een vraag en op basis van het antwoord, wordt de volgende server gecontacteerd die meer kan vertellen over het antwoord.



![Imgur](https://imgur.com/xdWxHL5.png)



> **In de praktijk wordt vaak een combinatie van beiden gebruikt** waarbij de locale server bij sommige intermediated servers de vraag doorspeelt (recursief) en bij root en authoritative servers een antwoord krijgt maar de vraag niet doorspeelt (iteratief).

![Imgur](https://imgur.com/0rjaTNK.png)



3. **Wat is (in de context van DNS) : RR, A, NS, CNAME, MX (geef ook een voorbeeld)**

>**RR** = Resource Records → Manier om informatie op te slaan in de DNS-database. De algemene syntax is: [name], [TTL], [class], record type, record data. Een Resource Record kan een A-, NS-, CNAME-, MX-… als recordtype bevatten.
>A = een IPv4 adres voor een corresponderende hostname (dns.google.com IN A 8.8.8.8)
>**NS** = name server → bevat het IP adres voor dat (sub)domein (domein != hostname) (ugent.be IN NS ugentdns1.be)
>**CNAME** = bevat een alias (e.g andere naam) voor een record naarwaar het point ([www.ugent.be](http://www.ugent.be) IN CNAME ugent.be)
>**MX** = mail record → verwijst naar een mailserver voor dat domein. (mail.ugent.be IN MX 157.8.8.8)



4. **Wat is reverse DNS en hoe werkt het?**

> Doet een **vertaling** van een **IP-adres** naar een **domeinnaam**. Dit kan gedaan worden om bijvoorbeeld na te gaan of de afzender van de mail geen spam is. Hiervoor moet een **PTR-record** worden geconfigureerd. Het maakt ook gebruik van het in-addr.arpa domein en schrijft het ip omgekeerd om zo tot het juiste domein te raken.



5. **Leg het principe van sockets uit en hoe je in de praktijk de actieve sockets op je Linux-**
   **systeem kan opvragen.**

>Sockets zijn de **actieve poorten** waar het systeem op luistert en eventueel verkeer ontvangt en/of verstuurd. Applicaties en/of services kunnen beroep doen op poorten om verkeer uit te sturen en te ontvangen. Het is als het ware een commnunicatie endpoint dat werwijst naar de "deur" tussen de **applicatie process** en het **end-to-end transport protocol**. Je kan actieve sockets opvragen met het **netstat** commando. Een modernere variant is “ss” maar werkt enkel op Linux.



![Imgur](https://imgur.com/aMBlNYy.png)

6. **Bespreek de werking en functie van een DHT in peer to peer netwerken.**

> DHT = Distributed Hash Table.

> Het gebruikt een hash tabel die gedistribueerd is over het p2p netwerk (dit is veel beter dan een tracker). **Peers** kunnen inserten in de tabel en ook queryen. Data wordt opgeslagen op de peers met hetzelfde id als de hash key. zo worden stukken van een bestand over een hoop deel peers verdeeld, als een id niet bestaat gaat het naar de opvolgende grootste peer. Door **peer churn** worden er een groot deel peers altijd aangemaakt en weer verwijderd. Hierdoor moet het altijd herverdeeld worden. Als een node weggaat geeft het zijn data door aan zijn opvolger. Je kan niet heel het netwerk opvragen over wie welke id heeft, dat wordt opgelost door de regel dat je gewoon altijd aan de opvolger moet vragen. In het slechtste geval moet de vraag heel de ronde passeren alvorens te weten over welk id het gaat. De oplossing hiervoor zijn **Finger Tables**.



## Hoofdstuk 4



1. **Leg NAT uit aan de hand van een voorbeeld. Bespreek de voor en nadelen van NAT.**

> NAT = Network Address Translation
>
> NAT (lange naam NATv4) laat toe om IPv4 adressen te vertalen naar een ander IPv4 adres. Deze vertaling kan zijn een private IPv4 adres naar een publiek IPv4 adres. Maar ook even goed een vertaling van een private naar een private IPv4 adres. De vertaling hoeft niet een 1-op-1 vertaling te zijn. Een range van IPv4 adressen (vb. 192.168.0.0/24) kan verwijzen naar één (publiek) IPv4 adres.
>
> Het werkt aan de hand van de poorten op een router. Dat laatste komt vooral voor in Azië waarbij de ISP’s een dubbele NAT-vertaling doen om ervoor te zorgen dat er genoeg IPv4 adressen beschikbaar zijn voor de consumenten. Zo wordt één publiek IPv4 adres vertaald naar een privaat adres dat zich in de 10.0.0.0/8 range bevindt. Nadien wordt dat adres uit de 10.0.0.0/8 range opnieuw vertaald naar een IP uit een andere range (vb 192.168.0.0/24). Dit IP is toegekend aan een endhost. Op die manier kunnen minstens 16 000 verschillende particulieren en bedrijven gemapped worden naar één publiek IPv4 adres.
>
> Het **voordeel** van NAT is dat het indirect kan fungeren als een firewall waarbij bepaalde IP adressen of ranges van IP adressen toegelaten/geweigerd kunnen worden om verbinding te maken met een ander adres-range of internet. Daarnaast kan het lokaal netwerk onafhankelijk veranderen zonder dat de buitenwereld hiervan op de hoogte moet worden gebracht.
>
> Het **nadeel** van NAT is dat het enkel werkt op layer 3 (op router-niveau). Verder zijn de entries ook enkel gebaseerd op uitgaand verkeer.



2. **Stel een packet flow diagram op, waar één client in een netwerk een DHCP adres aanvraagt, maar waar er twee DHCP servers in het netwerk voorkomen. Leg aan de hand hiervan de werking van DHCP uit.**

![img](https://lh6.googleusercontent.com/kJa5nV44E-NiDLsp4Xpp7m3vOmKRNPBYdYd-wIaFjiOOf4l5KB96ekQncEeRyyFEs554gBz3xQYaNPyf9ekkMw5yT8KlUKjrISF7AkxUYLQdHkJ-RFNVR7j9nt27Smm0HODnSY6BcKk0F_vt6w)



>  De PC stuurt een DHCP-DISCOVER bericht uit op het netwerk. Dit is een broadcastbericht, wat betekent dat iedereen binnen het netwerk het bericht zal ontvangen.Beide servers ontvangen het DHCP-DISCOVER bericht waarna beide servers een DHCP-OFFER bericht sturen (als ze nog beschikbare IP adressen hebben om uit te delen). Dit bericht wordt in unicast formaat uitgestuurd, wat wil zeggen dat het bericht naar één iemand wordt gestuurd. Dit betekent dat de PC 2 DHCP-OFFER unicast berichten zal ontvangen. 
>
> De PC kiest zelf aan welke server die een DHCP-REQUEST (unicast) bericht die zal uitsturen, vaak is dat de server waarvan de PC het eerst een DHCP-OFFER bericht van heeft gekregen. De PC stuurt een DHCP-REQUEST bericht uit met de vraag om dat IP-adres te verkrijgen.
> De DHCP-server zal antwoorden met een DHCP-ACKNOWLEDGEMENT bericht waarna de computer voor een leasetijd (meestal 24 uur) dat IP-adres toegeëigend krijgt.



**3. Wat is DHCP relay en waarvoor dient het?**

> DHCP relay zorgt ervoor dat DHCP-requests de lokale LAN kunnen verlaten om een DHCP-server te kunnen bereiken die op een ander subnet gelegen is. **De DHCP Relay agent** stuurt alle broadcast messages door naar een DHCP server in een ander subnet. Hij ontvangt een **DHCPDISCOVER** **broadcast** message, ze het om naar een **UNICAST** message en verstuurt het daarna door naar een DHCP server in een ander subnet. Daarna stuurt de externe DHCP server een **DHCPOFFER** **unicast** terug. De relay agent **broadcast** vervolgens het **DHCPOFFER** binnen zijn subnet.



![img](https://lh3.googleusercontent.com/kSRACYlSY4SNyJVS5dMAj5shEb4cTsE_VPwbAM1yxWGvugoxQ36q9iOQfzfrYSuY9qt89lrMMuAWFhdIbqhHp3d7jA9RSBdap4nl8D7ULuI9dMI1kkNTNbWol6G4RWfCUri9waNvGJeNCU_OvA)



4. **Leg het werkingsprincipe uit van Software-Defined Networking. Wat zijn de voordelen.**

> SDN (Software-Defined Networking)
>
> Het is een benadering van netwerkbeheer die dynamische, programmatisch efficiënte netwerkconfiguratie mogelijk maakt om de netwerkprestaties en -security te verbeteren, waardoor het meer lijkt op cloud computing dan op traditioneel netwerkbeheer.Het voorziet abstracties om op basis daarvan routing applicaties te schrijven.
> OpenFlow is de pionier binnen dit concept.. Dit is de taal die gesproken wordt tussen de control plane (Netwerk OS) en de data plane.![img](https://lh5.googleusercontent.com/XBHHpHALovtr2H-ecTyBdoWv2qUnzmTeTpQ6N7vHZWVm8UcTGjFx0lsP3Ainu6XgGru18bGfwW6WErgctdWJHG1jgy3zOskEjN0FpMtm0p4yx9KHmf2EJdWGoakjDXLYeWFaGNeqSolVdy7svQ)





## Hoofdstuk 5

**1. Wat is een AS ? Geef 3 types (waarom is het belangrijk een onderscheid te maken).**

>  AS = **Autonomous System**
> Een AS is een groep routers/netwerk elementen binnen dezelfde administratieve controle (beheerd of eigen van hetzelfde bedrijf, universiteit of provider).
>
> **3 types:**
>
> - Stub AS: Heeft slechts 1 connectie naar de rest van het internet
> - Transit AS: verbindt 2 of meerdere AS’sen.
> - Multi connected AS: heeft via meerdere verbindingen toegang tot andere AS’sen maar fungeert niet als transit.
> - ![img](https://lh3.googleusercontent.com/EoezP7FO7bGHEykisYL8ELQ2YdlVH02F_Yf2iu1DIDCEidXpw30NyLpDVtssgmpUI7p5NtaV7kOI5qFe5KXgNMUjoJlxSCEcMA8i4zrqWuNkD5X3jA8a1zFcjtzr8htTlKbwHe9VVI_VRIGjDg)

**2. Bespreek het verschil tussen intra- en inter-AS routering.**

> **intra-AS** routering: binnen AS (via **OSPF** of **RIP**)
>
> **![img](https://lh5.googleusercontent.com/3hyOQp_hw3JVmQZz6M6VEU_8deVmbm9F6FPcAVUajJbPw-gzdQURokG-oL4pDAiHhfzpm0YYcAnEBsKNcJcbEiLvkTHsiTlfgmmjn1UXLbjdksPmUvHur3CXv3MkbUw8FwsLPDSBYmk-Se6CVg)**
>
> **inter-AS** routering: tussen AS’sen (via **BGP**)

**![img](https://lh3.googleusercontent.com/LxU9_lUJKLMlkkFPtLSLGCbmapvfYmJCxPQUZNyX6_YeRKSC9w94_tfzm_oHQENpwz_FpPld_-ayoQGXyHWqYP0yP7ZxOkvY9Tp7fDeZpWRXV4k-6FISDSdj4gejACE7W1jiMlWnMVaSfMC-og)**



**3. Leg het werkingsprincipe van distance vector en link-state routering uit. Geef een voorbeeld voor beide strategieën.**



> **distance vector routing:** disctance vector = vector met gewichten die aangeven hoe ver een gegeven router zich bevindt (bepaald door bandbreedte). Afstand wordt bepaald door Bellman-Ford algoritme.
> Iedere router ontvangt disctance vector van buren (bv elke 30 seconden). Ook update hij zijn eigen locale distance vector.
>
> **![img](https://lh5.googleusercontent.com/2PhqlEdbts0km3WiKjTGpcJ5xt-ShXNU0GrGHJmaZ0hog1QgB4nngs6bg0G-xfSPM_0UOxmd8NhLoxWQKdHBrYV0fn-Xn4i5tZFQ4uf7t1qctO-dkkKuagO3tlF-Kn2K04KJ-EQybKhk4MSHAQ)**
>
> 
>
> **link-state routing:**
> Houdt afstand tot bepaalde router bij aan de hand van een link state database. Maakt gebruik van advertisements om link states te verspreiden.Maakt gebruik van OSPF om het shortest path te bepalen.
> ![img](https://lh4.googleusercontent.com/GjkDpefkPTyMTGttAzBr8SHhYF9BtgBK82Muk2R70E_-HL59ONX1y-1_OC6xlAe3Olaub99jd61DRAg8PNAiTZORvca4Kp5IkFP924CIiQBSm5ZLxJCcfaT0TPW2MwZZnXDtvhI4Rclq1JS9_A)





**4. Hoe gaan distance vector en link-state routering om met problemen in het netwerk?** **Evalueer de voor- en nadelen van beide strategieën.**

> **distance vector routing:** 
>
> - **Count to infinity** :	
>
>   Oplossingen:
>
>   - **Split horizon**: Als router nooit distance vector doorgeven aan buur als router gebruik maakt van buur om bestemming te bereiken.
>   - **Poisoned reverse** : Als router nooit distance vector doorgeven aan buur als router gebruik maakt van buur om bestemming te bereiken.In plaats daarvan geef je afstand oneindig mee.
>
>    **link-state routing:**
>
> - Geen directe problemen (denk ik?).



**5. Bespreek hierarchical OSPF. Waarom is dat nuttig?**



> OSPF = Open Shortest Path First

>- zowel IPV4 als IPV6

>- Wordt gebruikt bij intra-AS routing

>- maakt gebruik van advertisements om link states te verspreiden Dit doet hij over volledige AS adhv flooding. OSPF stuurt direct een IP pakket in plaats van TCP of UDP

>- advanced features:
>  - Security (authenticatie)
>  - multipath : verschillende same-cost paths toegestaan
>  - uni- & multicast support
>  - hiërarchische OSPF binnen grote domeinen![img](https://lh4.googleusercontent.com/yAgc1zCsd5oKkJZu4M8_oGEOs17smJ8kKgCj2JCkaJgCm8TTHWi3S480EOOahGNa4bODmKmCvyq-zGom2Pq_1A3rjs0w3SGzcx8va9T0pDZIbg7erGzu6oxFdr9avSQRKzoOB6ufMEicPZg-4w)

  

**6. Bespreek een voorbeeld van BGP. Waarom heeft men I-BGP en E-BGP ?**

> BGP = Border gateway protocol.
>
> Wordt gebruikt om te routering tussen verschillende AS’sen gebruik makend van tcp (port 179). Het is een path-vector protocol (deelt paden naar andere AS’en met verschillende tussenliggende AS’en.) bv D via [A,B,C]. 
>
> **principes**: 

>- advertisement:border routers delen subnetten die ze kunnen bereiken met andere border routers. (via **E-BPG** = external BGP)
>- propagation:
>  - **I-BGP** (internal BGP) wordt gebruikt om reachability informatie te delen met andere routers binnen AS.
>  - policy
>- route selection: adhv van volgende eliminatie regels:
>  - policy
>  - #aantal AS’enclosest 
>  - next-hop (berekend door intra-as protocol)

**7. Wat is een AS-PATH ? Wat is een NEXT-HOP ?**

> Het zijn BGP-attributes**.**
>
> - AS-PATH: path vector, welke zijn de tusseliggende AS’sen?
> - NEXT-HOP: Interface waar AS-path begint. Hops zijn geen routers maar AS’en.





**8. Wat is policy based routing in BGP ?****Geef een voorbeeld.**

>  Border routers gaan informatie gaan delen met andere router aan de hand van bepaalde voorwaarden (policies). 
>
> Zo zal AS 54 in onderstaand voorbeeld zijn informatie niet delen met AS 134 omdat AS 134 geen betalende klant is.
>
> ![img](https://lh3.googleusercontent.com/XgPPDsWLWg4rhOKRRd3EfeCyi7PnWRKjD0fhIdRVk5gBg_qz8Lm5_6TLOiEO5mYrcRT80GYKelRQohfNGsxEcIAYKjnOr8TNb_n8QMG-hRfDjxzUrQESvlcXNaFjvqufrOMGlxbEvVhmF2Iu7A)



**9. Wat is ICMP ? Geef een voorbeeld bij het gebruik in een redirect en traceroute.**

> ICMP = Internet Control Message Protocol. Het wordt bijvoorbeeld gebruikt om te achterhalen of een host bereikbaar is. Hiervoor kan het ping- of traceroute-commando worden gebruikt. Sommige firewalls blokkeren ICMP berichten om te voorkomen dat hackers hier misbruik van maken om bijvoorbeeld een DDOS-aanval mee uit te voeren.

> **Redirect** staat in voor het detecteren en oplossen van routeringsproblemen. Dit aan de hand van ICMP redirect berichten.(Voorbeeld:..)

> Via **traceroute** kunnen de IP-adressen van de tussenliggende hops tussen de source- en destinationhost worden weergegeven. Hiervoor wordt er steeds een ICMP bericht uitgestuurd die bij elk nieuw pakket, de Time To Live (TTL) met één verhoogd. Dit wordt gedaan totdat de destinationhost bereikt is. Wanneer de host niet bereikbaar is binnen de maximale TTL (maximum?), zal het antwoord een timeout teruggeven. De TTL kan ook beschreven worden met het maximum aantal tussenliggende hops het pakket mag passeren om de destinationhost te bereiken.

**10. Leg de basisprincipes van SNMP uit (wat, waarom, hoe, ...). Verwerk het woord MIB en OID in je antwoord.**

>SNMP = Simple Network Message ProtocolHet Object ID (OID) is het ID in de MIB-tree dat een hiërarchische ordening heeft.MIB = Management Information Base
>
>Het Simple Network Message Protocol (SNMP) laat toe om UDP-berichten uit te sturen naar beheerde toestellen zoals routers, switches, servers… om informatie op te vragen of configuratiewijzigingen door te voeren.
>
>De Management Information Base (MIB) wordt gebruikt om informatie bij te houden over de network management data. Om veranderingen in het netwerk door te voeren, moet de managing server SNMP berichten verzenden naar de agenten van de desbetreffende nodes om de wijzigingen te kunnen doorvoeren. Hiervoor moet de Object ID (OID) bij de MIB gekend zijn.
>
>De managing server kan ook informatie over de nodes opvragen. Dit moet gebeuren op basis van het OID. Ook hier wordt een SNMP bericht gestuurd. De agent van de node stuurt hierop een antwoord terug. De MIB houdt een hiërarchische boomstructuur bij van de objecten. Elke branche in de boom bevat een naam en getal. Door getallen, scheiden met een “.” op te geven, kom je uit op een OID. 



**11. Hoe kan Ansible ingezet worden voor netwerkautomatisering, en wat zijn de nodige basiscomponenten en protocollen van Ansible?**

>Ansible bevat playbooks die configuratie bevatten voor desbetreffende hosts die in de playbook gedefinieerd zijn, die op hun eigen een verwijzing zijn naar de hostfile. Zo kan de configuratie worden ingesteld op honderden hosts tegelijkertijd.
>Het laat toe om frequent wijzigingen door te voeren op veel instanties door één playbook te runnen.