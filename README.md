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

After dumping a tag the structure of a brand new tag is something like this:

```
Sector 0: Containg manufacturing data and UID
Block 0: XX XX XX XX XX XX XX 00 02 A6 XX XX XX XX XX XX
Block 1: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 2: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 3: FF FF FF FF FF FF FF 07 80 69 FF FF FF FF FF FF

Sector 1: This sector contains information about Build Tray Usage.
I was able to successfully reset the tray usage to 0% by manipulating this sector.
Block 4: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 5: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 6: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 7: XX XX XX XX XX XX FF 07 80 69 FF FF FF FF FF FF

Block 8: 05 41 73 69 67 61 00 00 00 00 00 00 00 00 00 00
Block 9: 0F 4E 6F 74 20 57 72 69 74 74 65 6E 20 79 65 74
Block 10: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 11: XX XX XX XX XX XX FF 07 80 69 FF FF FF FF FF FF

Block 12: 15 07 05 0C 04 01 00 07 A1 20 00 07 AA E4 00 00
Block 13: 05 41 73 69 67 61 00 00 00 00 00 00 00 00 00 00
Block 14: 0F 4E 6F 74 20 57 72 69 74 74 65 6E 20 79 65 74
Block 15: XX XX XX XX XX XX 07 8F 0F 69 FF FF FF FF FF FF

Block 16: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 17: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 18: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 19: XX XX XX XX XX XX FF 07 80 69 FF FF FF FF FF FF

Block 20: FF FF FF 7F 00 00 00 80 FF FF FF 7F 14 EB 14 EB
Block 21: FF FF FF 7F 00 00 00 80 FF FF FF 7F 15 EA 15 EA
Block 22: FF FF FF 7F 00 00 00 80 FF FF FF 7F 16 E9 16 E9
Block 23: XX XX XX XX XX XX 77 88 78 69 FF FF FF FF FF FF

Block 24: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 25: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 26: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 27: FF FF FF FF FF FF FF 07 80 69 FF FF FF FF FF FF

Block 28: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 29: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 30: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 31: FF FF FF FF FF FF FF 07 80 69 FF FF FF FF FF FF

Block 32: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 33: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 34: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 35: FF FF FF FF FF FF FF 07 80 69 FF FF FF FF FF FF

Block 36: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 37: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 38: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 39: FF FF FF FF FF FF FF 07 80 69 FF FF FF FF FF FF

Block 40: 6B 9A EF CD C6 11 97 85 93 6F B0 A8 B6 60 6E BE
Block 41: 0D E4 23 C1 62 0F 85 B2 5A 10 D5 CF EF 42 01 68
Block 42: 92 83 90 6D 42 48 58 A5 F1 73 85 83 1C 77 B2 DC
Block 43: XX XX XX XX XX XX 07 8F 0F 69 FF FF FF FF FF FF

Block 44: 41 1D 4E B0 B0 B6 DC B9 2D 48 E0 17 30 00 7E B7
Block 45: 68 B6 CF 51 A7 EE 38 35 84 F2 B0 16 6A A2 EC E6
Block 46: B7 AB 34 78 8A 05 62 DF 7B AF 3F BF D6 EE 0D 83
Block 47: XX XX XX XX XX XX 07 8F 0F 69 FF FF FF FF FF FF

Block 48: CE D9 22 BF D9 CF 0A BA 0C 3D 39 30 73 32 00 D1
Block 49: 30 15 51 88 3A F7 16 0D 3E 0C B2 93 6B 55 94 E2
Block 50: 11 A6 61 59 9D 24 7C F2 8C CA AA 07 04 44 E4 F8
Block 51: XX XX XX XX XX XX 07 8F 0F 69 FF FF FF FF FF FF

Block 52: 48 7D F8 D7 BB D0 AA AC CB DA 3E BC 51 23 D1 B6
Block 53: 0A 82 52 34 E2 ED 78 EB CF E8 70 B2 D0 45 18 10
Block 54: 9E 58 24 8E E1 15 52 8B C1 6E EA 05 9E 94 B7 DE
Block 55: XX XX XX XX XX XX 07 8F 0F 69 FF FF FF FF FF FF

Block 56: 8C 0E 35 71 35 B5 23 EC 4D 80 00 54 6E A0 A9 B3
Block 57: 34 39 A6 58 13 CB E8 D8 CA D6 27 51 F2 9E 19 E0
Block 58: 1F 4E EC 37 37 E3 72 DC 0C ED 80 48 03 05 99 6B
Block 59: XX XX XX XX XX XX 07 8F 0F 69 FF FF FF FF FF FF

Block 60: 1B E9 2F 00 F8 CB F9 D0 E9 F7 45 2F 1B 49 5B 42
Block 61: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 62: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Block 63: XX XX XX XX XX XX 07 8F 0F 69 FF FF FF FF FF FF
```
