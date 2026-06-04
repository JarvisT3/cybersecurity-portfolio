AUTHOR'S NOTE:  FOR POLICY ADHERANCE REQUIREMENTS SOME ITEMS IN THIS CTF WRITE UP ARE <REDACTED>. 

The Spring NCL CTF challenge provided an image with what appeared to be cipher text of some sort.  The intent appeared to be for decoding the ciphertext or identifying a red herring element.  At first glance it appeared to simply be a matter of decoding the symbols, possibly more tasking regarding verifying the cipher.  

Approach Summary
I utilized Linux and PowerShell command line tools, online tools, image search, google queries, and online research.  My theory was premised on the cipher appearing to be a substitution cipher.  This led me to discovering it’s a <redacted> cipher via reverse image searching and the ‘dors’ context clue pointed to Diana Dors, English actress, which was discovered by doing Google searches for ‘dorsCrypt’ and ‘pigpen cipher’ terms.  I located a cipher decoder / encoder tool online and attempted to map the ciphers and decode.  I thought this would be simple with a decoder, but it quickly turned against me, where I struggled to identify the “F” looking character that I couldn’t identify if it was legitimate or a red herring.  Using the decoder, I attempted to analyze if the cipher was a specific variant, uncovering that it appeared to be variant 2 of pigpen. 

Tools

Tools & resources used 
File
Exiftool
Binwalk
Hexdump
Identify
Grep
Strings
Gimp
Cyberchef
Google image search / reverse image search
Dcode.fr/pigpen-cipher
Detailed techniques attempted 
File: the File CLI tool was used to ensure the file extension shown was binarily the same file type.  This utility also helped me ensure the file properties ( file type, dimensions, encoding, EXIF presence, color components, etc ) were commensurate with the file type.  This particular file appeared cropped or resized, as the file command revealed two dimensions but further investigation with identify, binwalk, and exiftool command line tools confirmed dimensions. 
Commands:
	File ciphertext.jpg // outputs file attributes
o	 
Exiftool: The exiftool command line tool was useful for identifying potentially revealing metadata.  Metadata can often reveal layered context clues such as GPS data, anomalies in image size, color schemes, similar to the file CLI tool, which may reveal artifacts such as a location that progresses you toward the next clue, compression oddities, or color scheme info that may reveal a need to modify color components or scheme. 
Commands:
	Exiftool -a -u -g1 ciphertext.jpg // retrieves duplicate values, unknown attributes, and outputs in single-header format; assists in locating anomalies or revelations of context clues in the metadata.
o	 

Binwalk: Because binwalk has the ability to identify embedded files, this is a natural next step.  Ensuring embedded files aren’t present is necessary to verifying the file or image itself isn’t a red herring / obfuscation.  Binwalk is also useful for locating secondary data streams, secondary file signatures, encryption blobs, and steganographic payload containers. 
Commands:
	Binwalk ciphertext.jpg // assesses for embedded files
o	 

	Binwalk -B ciphertext.jpg // scans for raw signatures
o	 

	Binwalk -e ciphertext.jpg // verification attempt to extract embedded files
o	 

	Binwalk -E ciphertext.jpg // checks entropy graph info to rule out byte randomness or encryption
o	 
Grep:  I thought using grep might be useful but running it against the file itself wasn’t revealing, as I expected, but I believe you should always check even the low hanging fruit items.  However, using grep on the hexdump output allowed me to capture the important binary details that confirmed start and end of the file. 
Commands:
	Grep -i sky ciphertext.jpg // case-insensitive search for a string containing ‘sky’
o	 
	 
Hexdump: hexdump was used to extract the hex data from the file to look for magic bytes, appended data, and confirm jpg structure markers.  After placing the hexdump results into a text file I searched for artifacts:  Magic bytes:  ff d8 ff e0 // start of image, header validation;  End of imager marker:  ff d9 // observe the presence of anomalies, artifacts AFTER the end marker. 
Commands:
	Touch dorsCrypt_hexdump-X.txt // creates text file for placing output of hexdump
	Hexdump -X ciphertext.jpg > dorsCrypt_hexdump-X.txt // places hexdump output into text file
	 
	 

Identify: I used this ImageMagick CLI utility for further confirming file details, specifically the color scheme, as it provides an incredibly in depth view of the color attributes.  My thought here was to see if there were any color schemes with anomalous characteristics and conduct an LSB analysis if anomalies existed. 

Commands:
	 
	   // example of color statistics from CLI output

Strings: running strings on the file to identify ASCII characters of potential value.  This another low hanging fruit approach on image files, which produced nothing of value on this challenge. 

Gimp: Gimp was used to view the image with an image viewer / processing tool capable of reviewing the presence of layers or hidden items behind a transparency filter. 
Processing:
	   // no additional layers
	 
o	Opacity filtering didn’t reveal an image hidden behind the primary image.


Cyberchef: to verify my assumption there was no LSB to extract I uploaded the image to cyberchef, which confirmed my theory of no extractable LSB. 
	 
Google image search / reverse image search: this reverse image search method was the most revealing in terms of providing the biggest initial clue, as it was the method that revealed the cipher is pigpen. 
	 
Dcode.fr/pigpen-cipher: knowing the cipher, searched for a “pigpen cipher decoder”, which took me to https://www.dcode.fr/pigpen-cipher.  This is where I thought for sure I was going to solve it and I was able to make out a handful of words but kept getting hung up on the “F” looking character.  I tried multiple times to just use a few characters to make sure I was using the right variant and mapping rules, re-verifying back and forth with any internet searches on Diana Dors to see if stories of hers would reveal something extra I needed to do.  Well, I discovered after the game was over, when I ran the image through reverse search and enabled AI, I found my mistake – the image needed to be turned 180 degrees.  
Findings / evidence 
The CLI commands and cyberchef analysis turned up empty, although I learned more about image analysis using CLI tools, so something was garnered from my efforts.  As I was attempting to decode the cipher, the “F” character continuously deceived my efforts.  Each attempted input or substitution resulted in completely different character sets for the remaining characters.  My research into Diana Dors and the Freemasons turned up nothing meaningful for deciphering the “F” character variations.
