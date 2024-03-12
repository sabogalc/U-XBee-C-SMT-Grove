# U-XBee-C SMT Grove
This is a reverse-engineered Digi XBee Grove (SMT) with a USB-C connector. I will document how I made this board as well as what you should look out for if you are looking to do something similar.

Thankfully, lots of the documentation around this board is public, and I would not have been able to make this project without the documentation. That being said, some of the documentation is incorrect or mismatched, so I will aim to make some corrections in this write-up.

The first things I referenced were the [schematic and Gerber files](https://www.digi.com/resources/documentation/DigiDocs/90001457-13/reference/r_xbee_grove_development_schematic_smt.htm) directly from Digi, and I was excited at the prospect of being able to take these Gerbers and easily make a board from them. From there, I would just swap the USB port and call it a day, but unfortunately it was not this easy.

When I viewed the Gerbers, I noticed some weird artifacts in the bottom left corner that you can see below. These were present no matter what software I was using to view them.

![Screenshot 2024-02-04 174200](https://github.com/sabogalc/U-XBee-C-SMT-Grove/assets/53708281/7191024d-4969-4efe-82ab-bf125f330444)


On taking a closer look, I found what were some very usable Gerbers, but unfortunately I was not successful in reversing them to make a board in either KiCad or Altium Designer. I referenced [this guide](https://www.altium.com/documentation/knowledge-base/altium-designer/convert-gerber-odb-fabrication-data-back-to-pcb) for Altium and [this guide](https://forum.kicad.info/t/reverse-engineering-kicad-project-from-gerber-files/30903) for KiCad.

![Screenshot 2024-02-04 174232](https://github.com/sabogalc/U-XBee-C-SMT-Grove/assets/53708281/3b2b0120-c018-46a9-9f7c-b0907c9a3bc6)


After putting in some effort to get a board file from the Gerbers, I realized that I'd be better off making my own board from scratch while using the Gerbers as a reference. So I got to work on recreating the schematic and footprints.

The most difficulat part of this process was creating the footprint of the board shape and the SMT header pins around the rectangular hole at the top. I spent a long time figuring out what part these headers were as well as what footprint I should use for them. I found [this knowledge base post from Digi](https://www.digi.com/support/knowledge-base/what-type-of-header-is-used-on-the-xbib-u-ss-inter) that revealed the header pins to be a custom part with part number 76002056. This part number shows up in both DigiKey and Mouser, but the product is obsolete. However, even if it wasn't [this StackExchange answer](https://electronics.stackexchange.com/a/195601) indicates that the part cost $40 when it was available "Probably because some poor bastard at Digi whose official position is 'proprietary connector fabricator and needle nose plier operations chief' manufactures them on demand. By hand." That answer gave me a good laugh, but it's probably true.

Digi has a datasheeet for this part with the measurements [here](https://ftp1.digi.com/support/images/3405-SM-socket_hole-placement.pdf), but I personally would not trust the measurements from this document. I won't even go through the effort of correcting everything wrong with it, but the most immediate problem I found was that this document states that the rectangular hole measures 12.065mm x 17.145mm, while the Gerbers indicated that this hole was 14mm x 20mm. I recreated my footprint by following the Gerbers, and if you need a footprint for this part, just use mine. I combined the footprint for the board and the headers into one footprint called "XBEE_SMT" in the footprints library of this project.

![image](https://github.com/sabogalc/U-XBee-C-SMT-Grove/assets/53708281/2e412584-bb6a-4b63-8741-42568cee28ea)
>XBEE_SMT footprint from this project complete with the board shape, header pin holes, and fiducial markers.

The documentation for the board shape was a little better, but it still had a few issues. You can find the mechanical specifications [here](https://www.digi.com/resources/documentation/digidocs/pdfs/90001457-13.pdf#page=12), but they are incorrect by a factor of 0.04mm at the top. The corrected measurements are below. The documentation does not give measurements for the Non-Plated Through Holes in each corner of the board, but I got those from the Gerber files (the top two are 3.24mm in diameter and the bottom two are 3.2mm in diameter).

![IMG-1490](https://github.com/sabogalc/U-XBee-C-SMT-Grove/assets/53708281/a803064d-79b8-4916-82f4-c21fb4b380b6)

At this point, the hardest part was done, and I got to recreating the schematic. I was able to find [a bill of materials](https://media.digikey.com/pdf/Reference%20Design/Digi%20International%20Ref%20Designs/XBee_Grove_Dev_Brd_BOM.pdf) for this board (or for the THT version, I couldn't really tell), and this was very useful in obtaining footprints for parts such as the common mode choke or the potentiometer.

Notably, the BOM refers to the 4-pin Grove headers as ACC391450, but I could not find anything for this part number. 110990030 appears to be the exact same part, and this is the one I used for my project. Another discrepancy with the BOM is that it states the 10uF capacitors are size 0402/1005 Metric, but they appear to be 0603/1608 Metric at least, so I manually edited that. Additionally, the LTST-C190CKT red LED is obsolete, so I swapped it with a QBLP601-R5 LED if you were to actually make this with new parts and not just take them from an XBee Grove. Lastly, the part number given for the LED labeled "Green" in the schematic is a blue one, but this should not be a big deal.

With all of the parts imported and the schematic recreated (PDF version [here](https://github.com/sabogalc/U-XBee-C-SMT-Grove/blob/main/XBEE%20Grove%20C/XBEE%20Grove%20C.pdf)), the last thing to do was to route everything on the board. Once again, I did my best to reference the Gerber files and match the placement of the components, tracks, vias, and silkscreen. Once it was all said and done, I found my board to be quite similar to the original.

![XBee Comparison](https://github.com/sabogalc/U-XBee-C-SMT-Grove/assets/53708281/5a54a3d8-fb5a-4643-b0c1-06bdf915c136)

All in all, I am happy with how this project turned out, and if further improvements to the board could be made, I would be happy to implement them. Thanks for reading!
