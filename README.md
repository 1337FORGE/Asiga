# Security Analysis of Asiga Max 3D Printer: Cracking RFID Tags

### Introduction
The Asiga Max 3D printer has gained popularity in the dental field for its exceptional precision and versatility. My focus in this research was the RFID tags embedded in the printer trays. These tags contain crucial information that the printer relies on for seamless operation. I set out to test the vulnerability of these RFID tags and exploit any weaknesses I discovered.

### Important Note
Before I proceed, it is important to note that the information provided in this article regarding the dump information is purely for demonstration purposes. It does not represent actual tag data or reveal any specific vulnerabilities present in the Asiga Max 3D printer's implementation. The intention is solely to shed light on the inner workings of RFID tags.

### About the Tag

The RFID tag used in conjunction with the Asiga Max printer tray is a Chinese VM320708 paper tag. This tag operates in the High Frequency (HF) range at 13.56 MHz. Specifically, it belongs to the Mifare 1K 4-byte tag family.

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

To learn more about MF tags [click here](https://www.youtube.com/watch?v=RoiETfo_S4A).

### Encryption Analysis

- **Unique Key A:** After analyzing approximately 30 tags, it was observed that Asiga utilizes a unique Key A for each tag. Although a few duplicate keys were found, the overall analysis indicates that the keys are not susceptible to dictionary attacks. None of the keys in a dictionary of 3200 keys were found to match the Key A of any tag.

- **Default Key B:** Asiga employs the default key FFFFFFFFFFFF for Key B across all tags.

- **Vulnerability to Nested Attack (MFOC):** All keys examined during the analysis were found to be vulnerable to the Nested attack technique, also known as MFOC. This attack method allows for the cracking of the encryption keys used in the tags.

These findings highlight the importance of assessing and strengthening the security measures implemented in Asiga Max's RFID tags. The unique Key A implementation provides a level of protection against dictionary attacks.

### Tag Analysis Findings:




