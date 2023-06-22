# Security Analysis of Asiga Max 3D Printer: Cracking NFC (RFID) Tags

### Introduction
The Asiga Max 3D printer has gained popularity in the dental field for its exceptional precision and versatility. My focus in this research was the NFC (RFID) tags embedded in the printer trays. These tags contain crucial information that the printer relies on for seamless operation. I set out to test the vulnerability of these NFC (RFID) tags and exploit any weaknesses I discovered.

### Important Note
Before I proceed, it is important to note that the information provided in this article regarding the dump information is purely for demonstration purposes. It does not represent actual tag data or reveal any specific vulnerabilities present in the Asiga Max 3D printer's implementation. The intention is solely to shed light on the inner workings of NFC (RFID) tags.

### About the Tag

The NFC (RFID) tag used in conjunction with the Asiga Max printer tray is a Chinese VM320708 paper tag. This tag operates in the High Frequency (HF) range at 13.56 MHz. Specifically, it belongs to the Mifare 1K 4-byte tag family.

![VM320708](https://i.imgur.com/DcwnnsJ.png)

### About MiFare Classic 1K tags

Mifare Classic 1K tags are widely used in various applications, including the Asiga Max printer trays. These tags consist of 16 sectors, with each sector containing 4 blocks, resulting in a total of 64 blocks.

The structure of each block within a sector is as follows:

#### Block 0: Manufacturing Block (Sector 0)

- This block typically holds information about the tag itself, such as the Unique Identifier (UID) and other manufacturing-related data.

#### Block 1 to Block 63: Data Blocks (Sectors 0-15)

- These blocks store user data or information relevant to the specific application of the tag. The content and purpose of these blocks depend on the intended use case.

#### Each Sector comprises three important components:

- Key A: This is a 6-byte key used for authentication and access control. It is essential to provide the correct Key A to access the data stored in the block.

- Access Bits: These bits determine the access rights for the block, specifying who can read, write, or increment the value stored within. The access bits control the permissions and security levels associated with the block.

- Key B: Similar to Key A, Key B is another 6-byte key used for authentication and access control. It provides an additional layer of security and can be used in conjunction with Key A or independently, depending on the specific implementation.

I ended up making an NFC (RFID) reader with an Arduino

![RFID type reader](https://i.imgur.com/bmmiIMd.jpg)

To learn more about MF tags [click here](https://www.youtube.com/watch?v=RoiETfo_S4A).

### Data that the tag holds

According to the Tray Data section on the printer, these are the data that the tag holds:

Created: Date the tray created (or the tag)

First Used: ---

Build Counts: ---

Layer Count: ---

Build Tray Usage: ---

Minimum Firmware: ---

Capacity: ---

Max Capacity: --- 

Consumed: ---

Vendor ID: Asiga

Model: 000

![Asiga Tray Menu A](https://i.imgur.com/myg1b1s.jpg)

![Asiga Tray Menu B](https://i.imgur.com/ACXr2Jf.jpg)

### Encryption Analysis

- **Unique Key A:** After analyzing approximately 30 tags, it was observed that Asiga utilizes a unique Key A for each tag. Although a few duplicate keys were found, the overall analysis indicates that the keys are not susceptible to dictionary attacks. None of the keys in a dictionary of 3200 keys were found to match the Key A of any tag.

- **Default Key B:** Asiga employs the default key FFFFFFFFFFFF for Key B across all tags.

- **Vulnerability to Nested Attack (MFOC):** All keys examined during the analysis were found to be vulnerable to the Nested attack technique, also known as MFOC. This attack method allows for the cracking of the encryption keys used in the tags.

These findings highlight the importance of assessing and strengthening the security measures implemented in Asiga Max's NFC (RFID) tags. The unique Key A implementation provides a level of protection against dictionary attacks.

### Tag Analysis Findings:

After dumping a tag the structure of a brand new tag is something like this:

```
Sector 0
Block 0: Containing manufacturing data and UID
Block 1 to 3: Empty
```

```
Sector 1
Block 4 to 7:
This sector contains information about Build Tray Usage
I was able to successfully reset the tray usage to 0% by manipulating this sector
```

```
Sector 2
Block 8 to 11:
This sector seems to contain static information about the tray.
```

```
Sector 3
Block 12 to 15:
This sector seems to contain static information about the tray.
```

```
Sector 4
Block 16 to 19:
This sector seems to be related to the First Used sections as it signed a new value to the sector and locked it after.
I am not sure.
```

```
Sector 5
Block 20 to 23:
This section is dynamic too but I think it might be related to the percentage use. I am still not sure about this section.
```

```
Block 24 to Block 63 hold static information about the tag. I assume this information is things such as Vendor ID and other things.
```


### Conclusion
During my limited research with the Asiga Max printer and its NFC (RFID) tags, I discovered interesting aspects of how the printer interacts with the tags. It was observed that the printer reads the tag every 5-10 seconds and writes new information onto it. By the end of my investigation, I successfully cracked, extracted, and manipulated the data stored on the tag.

It is important to note that this research was conducted purely out of curiosity and for educational purposes. I did not have the opportunity to explore other printers utilizing similar principles or conduct further in-depth analysis. However, if I come across any additional printers with tags in the future, I will continue my research and share any new findings.

Thanks for reading
