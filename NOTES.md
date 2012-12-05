NOTES FOR GIGASET TINKERING
===========================

OpenWRT support
---------------

    v1       S30852-H70x-xxxx        no      original firmware version 2.x.x.x
    v1       S30852-S70x-xxxx        no      original firmware version 2.x.x.x, see: https://forum.openwrt.org/viewtopic.php?id=36612
    v2       S30852-S70x-xxxx        trunk, backfire with patch      original firmware version 4.x.x.x

in het geval van een v2 is er een grote kans om em aan de gang te
krijgen, in de andere gevallen wordt het moeilijker.

Info toestel
------------

Van de webinterface:

    Further information
    Date / Time:    01.01.0001, 00:31 Uhr
    Firmware Version:   7.2.0.0.1
    Boot code version:  Prim.: [4]1.22 (51.0) Sec..: [4]1.26 (52.0)
    ADSL Modem Code Version:    2.2.1.12.0.1
    Hardware version:   Q705-M321
    Serial Number:  0001E384A690
    
Model: Siemens Gigaset SX 763 WLAN dsl (testmodel!)

Interpretatie
-------------

Bootcode version:            Primary: 2.1.0.0.12     Secondary: 2.1.0.20.0

dit komt op eerste zicht overeen op de wiki met

   v1       S30852-S70x-xxxx        no      original firmware version 2.x.x.x, see: https://forum.openwrt.org/viewtopic.php?id=36612

in de zien we

Boot code version:  Prim.: [4]1.22 (51.0) Sec..: [4]1.26 (52.0)

wat dan overeen komt met

  v2       S30852-S70x-xxxx        trunk, backfire with patch      original firmware version 4.x.x.x

Links
-----

* http://wiki.openwrt.org/toh/gigaset/sx76x

Zeker te bekijken, source code repository van degene die het bootloader beveiligings systeem heeft omzeild.
Er staan kant en klare images op om dit te omzeilen (dus dit zou ook al geen issue meer mogen zijn)

* https://code.google.com/p/sx76x-openwrt-danube/

TODO
----
1. Gigaset openmaken en TTL kabels erop solderen (TX, RX en GND)
2. Deze aansluiten op een FTDi adapter of buspirate
3. Kijken of we output krijgen van de bootloader
4. Eventueel een backup nemen van de flash firmware/bootloader die er nu opstaat (met dd op de eerste /dev/mtdblock als we in de shell zelf geraken)
5. Kijken of het systeem hier vergelijkbaar is met wat op de wiki gezegd wordt (twee bootloaders, eerste controleert een checksum van de tweede, enkel geldig als die < 64k is)
6. Vervangen van de tweede bootloader met deze download https://sx76x-openwrt-danube.googlecode.com/svn/trunk/secondary-uboot/secondary_boot.img
7. Nadat de tweede bootloader is aangepast, herstarten en een openwrt binary uploaden via een nieuwe, aangepaste webinterface in die bootloader 

We hebben geluk, blijkbaar werd deze info recenterlijk nog aangepast: Last modified: 2012/06/24 14:22 by ewoutvonk
Het laatste dat ik gelezen heb was dat er een beveiliging op de op die bootloaders zat en toen was er nog geen directe oplossing voor.

Binaries
--------
Uit de backfire tag van OpenWRT zijn er verschillende binaries specifiek voor dit model gebouwd

http://downloads.openwrt.org/backfire/10.03.1/lantiq_danube/

openwrt-lantiq-danube-GIGASX76X-jffs2-128k.image   21-Dec-2011 07:06             3407876
openwrt-lantiq-danube-GIGASX76X-jffs2-256k.image   21-Dec-2011 07:06             3670020
openwrt-lantiq-danube-GIGASX76X-jffs2-64k.image    21-Dec-2011 07:06             3342340
openwrt-lantiq-danube-GIGASX76X-squashfs.image     21-Dec-2011 07:06             2424836

de laatste bevat enkel een squashfs partitie
de eerste drie zou je kunnen gebruiken met een package manager op de router zelf (ipkg of opkg)
de size in kilobytes achter de filenames geeft de grootte van de blocksector aan voor de Journalling Flash FileSystem partitie
Voor 8MB (wrs voor dit model, maar moeten we eens nakijken door de component te identificeren) zou dat de 128k versie zijn.

Mischien is het wel veiliger om eerste te beginnen met de plain squashfs versie.

Een and
