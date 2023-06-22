# Security Analysis of Asiga Max 3D Printer: Cracking RFID Tags

### Introduction
The Asiga Max 3D printer has gained popularity in the dental field for its exceptional precision and versatility. My focus in this research was the RFID tags embedded in the printer trays. These tags contain crucial information that the printer relies on for seamless operation. I set out to test the vulnerability of these RFID tags and exploit any weaknesses I discovered.

### Important Note
Before I proceed, it is important to note that the information provided in this article regarding the dump information is purely for demonstration purposes. It does not represent actual tag data or reveal any specific vulnerabilities present in the Asiga Max 3D printer's implementation. The intention is solely to shed light on the inner workings of RFID tags.

### About the Tag

The RFID tag used in conjunction with the Asiga Max printer tray is a Chinese VM320708 . This tag operates in the High Frequency (HF) range at 13.56 MHz. Specifically, it belongs to the Mifare 1K 4-byte tag family.

![VM320708](https://i.imgur.com/DcwnnsJ.png)

### Security of the tag
