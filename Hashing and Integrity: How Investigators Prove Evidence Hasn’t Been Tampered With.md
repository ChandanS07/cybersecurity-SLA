# Hashing and Integrity: How Investigators Prove Evidence Hasn’t Been Tampered With

## Introduction

Digital evidence is easy to copy and easy to alter. A single changed byte in a file may be invisible to a person, yet it can change what the file shows in court. Investigators therefore need a way to demonstrate that evidence today is identical to evidence at the moment it was collected.

The standard tool for this is the cryptographic hash. This article explains how hashing works, how forensic examiners apply it, and where its protection ends.

## Core Concepts

### What a hash function is

A hash function is an algorithm that takes data of any size and produces a short, fixed-size output called a hash value, or digest. The output works as a fingerprint of that data.

A cryptographic hash function has several defined properties:

- **Determinism:** the same input always produces the same hash, on any computer, at any time.
- **Fixed-length output:** the hash length never changes. SHA-256 always produces 256 bits, written as 64 hexadecimal characters. Hexadecimal is a base-16 notation that uses the digits 0–9 and the letters a–f.
- **Avalanche effect:** a very small change to the input, even one bit, changes the output so much that the new hash looks unrelated to the old one.
- **Pre-image resistance:** given only a hash, it is computationally infeasible to find an input that produces it. In other words, the function cannot practically be reversed.
- **Collision resistance:** it is computationally infeasible to find two different inputs that produce the same hash. Such a pair is called a collision.

### MD5, SHA-1 and SHA-256

MD5 produces a 128-bit hash and dates from 1991. SHA-1 produces a 160-bit hash. SHA-256 belongs to the SHA-2 family, standardised by NIST (the US National Institute of Standards and Technology), and produces a 256-bit hash.

MD5 and SHA-1 are discouraged because their collision resistance has been broken. Researchers showed practical MD5 collisions in 2004, and a practical SHA-1 collision was publicly demonstrated in 2017. NIST has deprecated SHA-1 for most security uses. SHA-256 has no known practical collision attacks, so it is the preferred choice.

## How Investigators Use Hashing

### Hashing at acquisition

Acquisition is the process of copying data from a device for analysis. The usual product is a forensic image, a bit-for-bit copy of the whole storage device, including deleted and unused space. The examiner hashes the original device and the image. If the two hashes match, the copy is exact. Both values are written in the acquisition log.

### Write blockers

A write blocker is a hardware or software tool placed between the evidence drive and the examiner’s computer. It allows read commands and blocks write commands. Without one, connecting a drive can alter data such as timestamps. A write blocker keeps the source unchanged, so its hash stays stable.

### Chain of custody

Chain of custody is the documented record of who handled evidence, when, why and where it was stored. Hash values are entered on this record at collection and at each transfer. A matching hash at every step supports the claim that nothing changed between hands.

### Re-verification

Examiners normally analyse a working copy and keep the original image untouched. They re-hash the image before analysis begins, and again before reporting and court.

### Known-file hash sets

A known-file hash set is a list of hashes of files already identified. The NIST National Software Reference Library publishes hashes of common software files. Examiners compare device files against such a set to exclude standard operating-system files and save review time. Other sets list known illegal or malicious files, allowing examiners to flag them without opening them.

## A Fictional Scenario

This case is entirely fictional.

A company suspects that an employee copied confidential files to a laptop. An examiner seizes the laptop at 10:15 on a Tuesday morning. She removes its drive, connects it through a hardware write blocker, and computes the SHA-256 hash of the drive. She then creates a forensic image and hashes that. Both hashes begin 9c41 and match in all 64 characters. She records both values on the acquisition form and signs it.

The image is sealed on an evidence server, and each transfer is logged with the hash.

Six weeks later, a second examiner re-hashes the image before analysis. It matches. Before the hearing, the defence asks for verification, and a third hash check again matches the original value. The examiner can state that the evidence analysed is identical to the evidence collected.

Had even one bit differed, the avalanche effect would have produced a completely different hash, and the mismatch would have exposed the change immediately.

## Limitations

Hashing proves integrity, not authenticity or origin. A matching hash shows that data has not changed since it was hashed. It does not show who created a file, when it was really made, or whether its content is true. A forged file hashes just as consistently as a genuine one.

Any change alters the hash, including harmless ones. Re-saving a document, or a single read error from a failing drive, produces a different hash. The mismatch shows that a change occurred, but not what changed or whether it was malicious.

Collision attacks affect weak algorithms. With MD5, an attacker who prepares two files in advance can make them share one hash, so a match would not prove the files are the same.

Finally, a hash is only as trustworthy as the process and records around it. A hash stored beside the evidence could be replaced along with it. Examiners therefore record hashes in signed, separately held documentation and follow written procedures.

## Conclusion

Hashing gives investigators a simple, repeatable test: if the hash matches, the data is unchanged. Combined with write blockers and documented custody, it lets examiners show that the evidence in court is the evidence that was seized.

It cannot prove where evidence came from or whether it is genuine. It supports, rather than replaces, sound forensic procedure.
