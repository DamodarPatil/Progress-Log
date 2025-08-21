---
title: "Hashing: Digital Fingerprints and Password Security"
date: 2025-08-21T17:05:11+05:30
draft: false
tags: ["cybersecurity", "hashing", "password-security", "cryptography", "tryhackme", "digital-forensics", "data-integrity", "hashcat", "john-the-ripper"]
---

So after wrapping up public key cryptography yesterday, I jumped straight into hashing today. I thought I had a decent understanding of what hashing was, but wow, was I wrong. This stuff is way more fundamental to cybersecurity than I realized.

The TryHackMe room started with a simple question that made me pause: "You just downloaded a 6GB file - how do you know it's exactly the same as the original?" My first thought was "uh, check the file size?" But that's obviously not enough. Enter hash values - basically digital fingerprints for data.

### The lightbulb moment with file integrity

Here's what clicked for me: a hash function takes ANY size input (could be a single letter, could be a 6GB file) and spits out a fixed-length string. And here's the crazy part - if even ONE bit changes in that massive file, the entire hash changes completely.

I tested this myself with two simple text files:
- One containing just the letter "T" 
- Another containing just the letter "U"

The difference? Literally one bit in binary. But their MD5, SHA1, and SHA256 hashes were completely different. That's the avalanche effect in action, and it's honestly pretty mind-blowing.

### Password storage finally makes sense

This is where things got really interesting for me. I always wondered why websites say they "can't tell you your password" when you forget it - they can only reset it. Now I get it!

They're not actually storing your password at all. They're storing the hash of your password. When you log in, they hash whatever you typed and compare it to the stored hash. If they match, you're in. If not, you're blocked.

It's brilliant because even if someone steals their database, they don't get actual passwords - just hash values that can't be reversed.

### The horror stories that made me paranoid

The room covered some massive security breaches that honestly made my skin crawl:

**RockYou (2009)** - They stored 14+ million passwords in plain text. Just sitting there, readable by anyone who got access to the database. The fact that this file (`rockyou.txt`) is now a standard wordlist in Kali Linux says everything about how badly they messed up.

**Adobe** - Used outdated encryption AND stored password hints in plain text. Sometimes the hints basically gave away the password anyway. 

**LinkedIn (2012)** - Used SHA-1 (now considered weak) with no salting. This made the hashes vulnerable to rainbow table attacks.

These aren't small companies making rookie mistakes - these are major tech companies that should know better. It really drove home how critical proper password storage is.

### Salting: The game-changer I didn't know about

Here's something I never understood before: why do two people with the same password get different hash values in secure systems?

The answer is salting. Before hashing the password, the system adds a random string (the salt) to it. So even if two users have "password123" as their password, their hashes will be completely different because they each get a unique random salt.

This prevents rainbow table attacks, where attackers use precomputed lists of common passwords and their hashes. With salting, those precomputed tables become useless.

### Hash cracking tools are terrifyingly effective

I knew tools like Hashcat existed, but I didn't realize how powerful they are. The fact that GPUs can try millions of password combinations per second is genuinely scary. 

What's interesting is that some modern hashing algorithms like bcrypt are specifically designed to be slow, even on GPUs. It's an arms race between security and attacking tools.

I tried to run some basic Hashcat examples in my VM, but as expected, VMs don't play well with GPU acceleration. Mental note: if I ever need to do serious hash cracking (for legitimate purposes!), I'll need to run it on the host OS.

### Beyond passwords: integrity checking everywhere

The more I learned about hashing, the more I realized it's everywhere:

- Every time I download a Linux ISO, there's a SHA256 checksum to verify it
- Git uses hashes to track changes and ensure repository integrity
- Digital forensics relies heavily on hashing to prove evidence hasn't been tampered with
- Even blockchain technology is built on hashing concepts

I actually went and checked some of my recent downloads against their published checksums. It's oddly satisfying when the hashes match perfectly.

### HMAC: Hashing with a secret sauce

Near the end, the room covered HMAC (Hash-based Message Authentication Code), which combines hashing with a secret key. This provides both integrity checking AND authentication - you can verify not just that the message wasn't changed, but also that it came from someone who knows the secret key.

The math behind HMAC involves XOR operations and double hashing, which initially looked intimidating. But breaking it down step by step, it's actually quite elegant. It's like putting a tamper-evident seal on a message that only certain people can create.

### The big picture is becoming clearer

What really struck me today is how hashing, symmetric encryption, and asymmetric encryption all work together:

- Hashing verifies integrity and stores passwords securely
- Symmetric encryption provides fast, bulk data protection
- Asymmetric encryption handles key exchange and digital signatures

Each piece solves specific problems, and together they create the foundation for secure digital communication. It's like finally seeing how all the puzzle pieces fit together.

### Some practical takeaways

This stuff isn't just theoretical for me anymore. I'm already thinking about:

- Setting up proper password hashing in any future projects (bcrypt, scrypt, or Argon2)
- Always verifying checksums when downloading important files
- Understanding what's happening when I see those SSH fingerprint warnings
- Appreciating why password managers are so much better than reusing passwords

### What's next

I'm planning to dive deeper into John the Ripper next - one of the most popular password cracking tools I keep hearing about. After learning all this theory about hash cracking and password security, I want to get hands-on experience with how these tools actually work.

I'm particularly curious about:
- How John the Ripper compares to Hashcat in terms of capabilities
- Setting up custom wordlists and rules for more effective cracking
- Understanding the different attack modes and when to use each one
- Maybe trying some CTF-style password cracking challenges

Should be interesting to see the practical side of all this hashing theory. Plus, understanding how attackers actually crack passwords will help me better secure systems in the future.

Another solid day of learning in the books. Time to let this all sink in before tackling the next topic!