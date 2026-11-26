# NetworkWalks-Week-3-Password-Craking
Password Cracking — John the Ripper (JTR), Johnny GUI &amp; NetworkWalks Online Tools Program: NetworkWalks Cybersecurity Training — Batch B084E Author: ARINDAM CHATTERJEE

1. Project Overview
This repository records two Week 3 practical modules, both performed against the same lab file (My Locked PDF1.pdf), supplied by NetworkWalks for this exercise only:

W3-PM1 — Password Cracking with JTR: recover the password of a password-protected PDF using John the Ripper (JTR) on the command line and Johnny, its graphical front end, on Windows.
W3-PM2 — Password Cracking with NetworkWalks Tools: repeat the same recovery using NetworkWalks' own browser-based Hash Calculator and Password Cracker, which run entirely client-side with no file upload to a server.
Both modules demonstrate the same underlying idea — extracting a crackable hash from a protected file, then testing candidate passwords against it — using two different tool chains so the mechanics can be compared.

2. What Is Password Cracking?
Password cracking is the process of recovering a password from its stored or derived form, usually a hash, rather than from the plaintext itself.

In simple terms:

A protected file does not store your password as plain text. It stores a hash — a fixed-length, scrambled value derived from the password. Cracking means trying candidate passwords, hashing each one the same way, and checking for a match.

This matters for two reasons:

It shows why weak or common passwords fail quickly: a password in a wordlist or a short low-complexity password can be matched within seconds to minutes.
It shows why cracking is probabilistic and time-bound, not guaranteed: a strong, unique password drawn from a large keyspace can make a dictionary or brute-force attack computationally impractical.
3. Encryption vs Hashing
Concept	Direction	Purpose	Example in this lab
Encryption	Two-way (reversible with the correct key)	Protect confidentiality; the original data can be recovered	The PDF's content is encrypted; it opens once the correct password is supplied
Hashing	One-way (not reversible by design)	Verify integrity / match a value without storing the original	The PDF's password is checked against a hash extracted from the file, not against stored plaintext
Cracking a password hash does not "decrypt" the hash. It works by re-hashing guesses and comparing the result to the target hash until a match is found.

4. Attack Types Relevant to This Lab
Attack type	How it works	Used in this lab?
Dictionary attack	Tries each word in a wordlist (e.g. common passwords, leaked password lists)	Yes — both JTR's default mode and the NetworkWalks Password Cracker's built-in list
Brute-force attack	Tries every possible character combination within a defined length/charset	Not used here — target password was weak enough for a dictionary match
Rule-based / mask attack	Applies transformation rules (capitalization, leetspeak, appended digits) to a base wordlist	Not used in this lab, supported by JTR's jumbo build
5. Scope and Lab Context
Item	Value
Target file	My Locked PDF1.pdf (supplied by NetworkWalks for this lab only)
Platform	Windows PC (Johnny GUI, NetworkWalks browser tools); JTR core also available pre-installed on Kali Linux
Activity	Hash extraction from an encrypted PDF, dictionary-based password recovery
Authorization	File explicitly provided by NetworkWalks as a training target; no production, third-party, or out-of-scope system was tested
Test limitation	No exploitation beyond recovering the documented lab password; findings are not generalized to any system outside this lab
6. Tools Used and What They Do
Tool	Purpose in this exercise
John the Ripper (JTR)	Command-line password cracking engine. Supports many hash and file formats (PDF, ZIP, Office documents, Unix/Windows credential hashes) and multiple attack modes (single, wordlist, incremental/brute-force).
Johnny	Graphical front end for JTR. Lets a user load a hash file, select an attack mode, and start/monitor a cracking session without typing CLI flags.
pdf2john (via Online Hash Crack's PDF Hash Extractor)	Extracts the crackable $pdf$... hash format from a password-protected PDF so it can be handed to JTR or Johnny.
NetworkWalks Hash Calculator	Browser-based tool that parses a locked PDF locally (client-side, no upload) and outputs the same $pdf$... hash format used by JTR.
NetworkWalks Password Cracker	Browser-based dictionary attack tool. Takes a $pdf$... hash and a wordlist (built-in or user-supplied) and reports the matching password, following the same logic as JTR.
7. Methodology
W3-PM1 — JTR / Johnny (Windows)
Install John the Ripper (jumbo build) and Johnny on the Windows PC.
Extract the PDF's crackable hash using a pdf2john-compatible tool (Online Hash Crack's PDF Hash Extractor was used here instead of a local Python/Perl script, since the jumbo Windows build does not ship pdf2john.py as a ready-to-run executable).
Save the extracted hash to a .txt file in the exact $pdf$... format, with no extra characters (e.g. a leading b' from a copy-paste artifact must be removed).
In Johnny: point the "John the Ripper executable" setting to john.exe, open the saved hash file via Open password file, then Start new attack.
Record the recovered password once Johnny reports a match.
Open the PDF with the recovered password to confirm it is correct.
W3-PM2 — NetworkWalks Hash Calculator & Password Cracker
Open the NetworkWalks Hash Calculator in a browser and upload the same locked PDF. The tool parses the file locally and returns a $pdf$... hash.
Copy the complete hash value.
Open the NetworkWalks Password Cracker, paste the hash, and select the built-in wordlist (or upload a custom one).
Start the attack and wait for a match to be reported.
Open the PDF with the recovered password to confirm it is correct.
Why run the same task twice?
Running the identical file through two independent tool chains is a basic verification step: if both an offline, well-established tool (JTR) and an independent online implementation agree on the same password, confidence in the result is higher than relying on a single tool.

8. Observations From This Lab
These are observations specific to the supplied lab file, not a general statement about PDF encryption strength.

The PDF's password was recoverable via a dictionary attack within the built-in/default wordlists of both tool chains, which indicates the password was a common or low-complexity value rather than a long random one.
Both JTR/Johnny and the NetworkWalks tools extracted the same $pdf$... hash structure from the file, confirming the two tool chains are interoperable (both follow the pdf2john/hashcat-compatible hash format).
The NetworkWalks Hash Calculator performs extraction client-side in the browser, so the PDF content itself is not uploaded to a server — only the resulting hash is handled afterward, by the user, in the Password Cracker step.
Encryption strength in this case was limited by password strength, not by the PDF encryption algorithm itself. A longer, non-dictionary password with mixed character classes would have made both dictionary attacks fail, requiring a much slower brute-force or mask-based approach instead.
9. Why This Matters (Defender's Perspective)
Weak or common passwords remain the easiest way in. Regardless of how strong the underlying encryption algorithm is, a crackable password undermines it entirely.
Hash extraction tools are widely available and easy to use, including free browser-based tools that need no installation. Protecting a file with a password-based scheme should assume an attacker can obtain the hash and attempt offline cracking without rate limits.
Dictionary attacks are fast against common passwords. Avoiding dictionary words, names, and simple patterns meaningfully increases the effort required.
Longer, high-entropy passphrases are the practical mitigation for password-protected documents, since they resist both dictionary and brute-force attacks far better than short complex-looking passwords.
Note on flags/evidence: the lab flags and the recovered password are intentionally not reproduced in this README, to avoid publishing answers for a shared training exercise. They are kept in the flags/ and evidence/ folders for personal record-keeping and instructor review only.
