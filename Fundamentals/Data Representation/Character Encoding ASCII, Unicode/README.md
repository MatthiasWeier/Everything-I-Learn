# Character Encoding: ASCII (7-bit, 8-bit) and Unicode

Character encoding is a system used to represent characters as numerical values in computers. This document provides a deep dive into the following key encoding systems: **7-bit ASCII**, **8-bit ASCII**, and **Unicode**, while also touching on other important encoding schemes.

## 1. ASCII (7-bit)

### Overview
ASCII (American Standard Code for Information Interchange) is one of the earliest character encoding standards. It uses a **7-bit binary number** to represent characters, which allows for **128 unique characters** (2^7 = 128). ASCII was initially developed in the 1960s and has since been the foundation for many other encoding systems.

### Character Set
The original 7-bit ASCII includes:
- **Control Characters (0–31):** These characters control hardware devices like printers (e.g., newline, tab).
- **Printable Characters (32–126):** This range includes letters (A-Z, a-z), digits (0-9), punctuation marks, and symbols.

### ASCII Table (7-bit)

| Decimal | Hex   | Binary    | Character | Description         |
|---------|-------|-----------|-----------|---------------------|
| 0       | 0x00  | 00000000  | NUL       | Null                |
| 1       | 0x01  | 00000001  | SOH       | Start of Heading    |
| ...     | ...   | ...       | ...       | ...                 |
| 32      | 0x20  | 00100000  | Space     | Space character     |
| 65      | 0x41  | 01000001  | A         | Uppercase 'A'       |
| 97      | 0x61  | 01100001  | a         | Lowercase 'a'       |
| 126     | 0x7E  | 01111110  | ~         | Tilde               |

**Note:** In 7-bit ASCII, characters are represented in the range from 0 to 127.

### Use Cases
- **Early computing**: Teletypes, communication devices, and early computers.
- **Protocols**: Widely used in internet protocols such as HTTP, SMTP, and FTP.

## 2. Extended ASCII (8-bit)

### Overview
**Extended ASCII** expands the original 7-bit ASCII to an **8-bit encoding scheme**, allowing for **256 unique characters** (2^8 = 256). This extension was necessary to support characters in languages other than English and to introduce additional graphical symbols.

### Character Set
Extended ASCII retains the original 7-bit ASCII characters (0-127) and adds an additional 128 characters (128–255). The extra characters are often used for:
- **Accented characters** (é, è, ç, ñ) for languages like French, Spanish, and German.
- **Graphical symbols** such as lines, boxes, and currency symbols.

### Extended ASCII Table (8-bit)

| Decimal | Hex   | Binary     | Character | Description         |
|---------|-------|------------|-----------|---------------------|
| 128     | 0x80  | 10000000   | Ç         | Latin capital letter C with cedilla |
| 130     | 0x82  | 10000010   | é         | Latin small letter e with acute |
| 144     | 0x90  | 10010000   | É         | Latin capital letter E with acute |
| 255     | 0xFF  | 11111111   | ÿ         | Latin small letter y with diaeresis |

### Variations of Extended ASCII
There is no single standard for extended ASCII. Different systems define their own 8-bit ASCII sets:
- **ISO/IEC 8859-1 (Latin-1)**: Commonly used in Western European languages.
- **Windows-1252**: A variant of Latin-1 with additional characters used in Microsoft Windows.

### Use Cases
- **Early text processing**: Many early personal computers used extended ASCII to support international characters.
- **Legacy systems**: Extended ASCII remains in use for compatibility with older systems and documents.

## 3. Unicode

### Overview
**Unicode** was developed to solve the limitations of ASCII by creating a universal character encoding system that can represent characters from nearly every language. Unicode is flexible, allowing **up to 1,114,112 code points**, far beyond ASCII's limit.

Unicode supports multiple encoding forms, including:
- **UTF-8**: Variable-length encoding (1 to 4 bytes), backward-compatible with 7-bit ASCII.
- **UTF-16**: Variable-length encoding (2 or 4 bytes).
- **UTF-32**: Fixed-length encoding (4 bytes).

### Character Set
Unicode includes characters for almost every writing system:
- **Latin alphabet** (A-Z, a-z).
- **Greek, Cyrillic, Arabic, Hebrew**, and many others.
- **Emoji** and special symbols.

#### Unicode Table Examples

| Code Point | Hex   | Character | Description                   |
|------------|-------|-----------|-------------------------------|
| U+0041     | 0x41  | A         | Latin capital letter A         |
| U+03A9     | 0x3A9 | Ω         | Greek capital letter Omega     |
| U+1F600    | 0x1F600 | 😀     | Grinning face (Emoji)          |

### UTF-8 Encoding
UTF-8 is the most common Unicode encoding on the web due to its efficiency and backward compatibility with ASCII.

| Character | Code Point | UTF-8 Encoding (Binary)    |
|-----------|------------|----------------------------|
| A         | U+0041     | 01000001                   |
| Ω         | U+03A9     | 11001110 10101001          |
| 😀        | U+1F600    | 11110000 10011111 10011000 10000000 |

- **1 byte** for ASCII characters (0-127).
- **2 to 4 bytes** for characters outside the ASCII range.

### Unicode vs. ASCII

- **ASCII** is limited to English characters and symbols.
- **Unicode** covers a vast array of global languages and symbols.
- UTF-8 is **backward-compatible** with ASCII, while UTF-16 and UTF-32 are not.

### Use Cases
- **Web development**: UTF-8 is the standard encoding for websites.
- **Modern software**: Almost all modern applications use Unicode to ensure broad language support.
- **Emoji and non-Latin scripts**: Unicode allows integration of emojis and symbols like mathematical operators and historic scripts.

## 4. Other Encoding Schemes

### EBCDIC (Extended Binary Coded Decimal Interchange Code)

- Developed by IBM for mainframe systems.
- Uses an **8-bit encoding** scheme.
- Not compatible with ASCII or Unicode.

### ISO/IEC 8859

ISO/IEC 8859 is a family of encoding standards that include various parts to support different language groups:
- **ISO/IEC 8859-1 (Latin-1)**: Supports Western European languages.
- **ISO/IEC 8859-5**: Supports Cyrillic scripts.

### Shift-JIS
- **Shift-JIS** is an encoding used primarily for **Japanese characters**.
- It uses a **variable-length encoding** with 1 or 2 bytes.

### Big5
- **Big5** is a **traditional Chinese character encoding** used mainly in Taiwan and Hong Kong.
- Similar to Shift-JIS, it uses **variable-length encoding**.

## 5. Conclusion

Character encoding is an essential aspect of data representation, from the basic 7-bit ASCII to the expansive Unicode system. While ASCII is limited to English and simple symbols, Unicode has become the standard for supporting diverse languages, symbols, and emojis in today's global computing environment. Understanding the different encoding schemes and their use cases ensures compatibility and proper character representation across platforms and systems.

## Further Reading
- [Unicode Standard](https://www.ascii-code.com/)
- [UTF-8 Encoding Explained](https://www.utf8-chartable.de/)
- [ASCII Control Characters](https://www.asciitable.com/)

