---
title: "Public Key Cryptography Finally Clicked"
date: 2025-08-20T11:03:30+05:30
draft: false
tags: ["cybersecurity", "cryptography", "public-key-crypto", "rsa", "diffie-hellman", "ssh", "digital-signatures", "pgp", "tryhackme", "encryption"]
---

Okay, so remember how I mentioned diving into cryptography yesterday? Well, I decided to tackle the TryHackMe room on public key cryptography, and honestly, my brain feels like it just went through a blender. But in a good way! 

I've always heard terms like "RSA" and "digital signatures" thrown around, but they felt like these mysterious concepts that only math wizards could understand. Turns out, with the right analogies, this stuff actually makes sense.

### The lightbulb moment with everyday security

The room started with this coffee shop analogy that completely changed how I think about online security. When you're meeting someone face-to-face, you automatically get:
- You know it's really them (you can see them)
- You know their words are authentic (coming from their mouth)
- You know the message isn't changed (direct conversation)
- You can keep it private (talk quietly, sit away from others)

But online? None of that happens naturally. Every message, login, file download - it all needs these same protections artificially built in through cryptography. That's why we need this stuff!

### The key exchange puzzle that kept me up

Here's what blew my mind: symmetric encryption is super fast, but how do you share the secret key without someone intercepting it? It's like trying to mail someone a locked box along with the key - anyone could steal the key during delivery.

The analogy that finally made it click was thinking of it like this:
- I want to send you a secret code (symmetric key)
- You give me a lock that anyone can see/use (your public key) 
- I put my secret code in a box, lock it with your lock, and send it
- Only you have the key to open it (your private key)
- Now we both know the secret code and can talk fast using symmetric encryption

This is literally happening every time I visit an HTTPS website. My browser is doing this dance with the server to establish a secure connection. Mind blown.

### RSA: When math becomes magic

I'll be honest - when I first saw the RSA math, I almost closed the browser tab. Prime numbers, modular arithmetic, exponents... it looked terrifying. But then I realized something: the basic idea is actually simple.

The security comes from a really hard math problem: it's easy to multiply two big prime numbers together, but incredibly hard to work backwards and figure out what those two primes were just by looking at their product.

I tried the example with small numbers:
- Take primes 157 and 199
- Multiply them: 157 × 199 = 31,243
- Now try to figure out what 157 and 199 were just by looking at 31,243

Even with small numbers, it's annoying. With 600+ digit numbers? Your computer would die before solving it.

### Diffie-Hellman: Creating secrets in public

This one really messed with my head. Two people can somehow create the exact same secret key without ever directly sharing it, even if everyone is watching their conversation. 

The math involves both people picking private numbers, doing some calculations with public values, sharing results, then doing more calculations that somehow give them both the same final secret. It's like mathematical magic.

I worked through the example several times because I couldn't believe it actually works. The fact that Alice and Bob can both arrive at the same secret number without ever sending it to each other still feels like a trick.

### SSH keys: Finally understanding what I've been using

I've been using SSH keys for months without really understanding what was happening under the hood. Now I get it:

When I run `ssh-keygen`, I'm creating a key pair. The private key stays on my machine (and I should NEVER share it), while the public key goes on the server. When I connect, my SSH client proves I have the private key without actually sending it over the network.

It's so much more secure than passwords, and now I understand why. Even if someone intercepts all the network traffic, they can't get my private key from it.

Also learned that I should probably add passphrases to my SSH keys. Apparently tools like John the Ripper can crack unprotected private keys if someone gets access to them. Mental note to fix that.

### Digital signatures: Proving authenticity

This concept took me a while to wrap my head around. A digital signature isn't just slapping an image of your signature onto a document. It's mathematical proof that:
1. You actually created/approved this document
2. Nobody has modified it since you signed it

The process involves hashing the document and encrypting that hash with your private key. Anyone can verify it by decrypting with your public key and comparing hashes. If they match, they know it's legit and unchanged.

I keep thinking about how this applies to software downloads, email verification, and even cryptocurrency transactions. It's everywhere once you know what to look for.

### PGP/GPG: Email encryption that actually works

I'd heard of PGP before but always thought it was too complicated for normal people. Going through the GPG key generation process wasn't nearly as scary as I expected. The hardest part was choosing between all the different key types and settings.

The workflow makes sense now:
- Generate a key pair
- Share your public key with people who want to send you encrypted emails
- Keep your private key safe and use it to decrypt messages

I'm actually considering setting this up for real. Might be overkill for most of my emails, but it would be cool to have truly private communication when needed.

### The bigger picture

What really hit me is how all these concepts work together. HTTPS uses asymmetric encryption for key exchange, then symmetric encryption for speed. SSH uses key pairs for authentication. Digital signatures verify software integrity. PGP handles end-to-end email encryption.

It's like discovering that all these things I use daily are actually part of one big interconnected security system. And the math, while intimidating at first, is based on some really elegant ideas.

### What's next

Tomorrow I'm diving into hashing - another fundamental crypto concept that I keep seeing everywhere but don't fully understand yet. From what I've gathered, it's like creating digital fingerprints for data, and it's crucial for things like password storage and file integrity checking.

I'm particularly curious about:
- How hashing differs from encryption (apparently it's one-way only?)
- Why even tiny changes in input create completely different hash outputs
- How password cracking tools like Hashcat actually work
- What makes some hashing algorithms secure while others are considered broken

Should be interesting to see how this connects with everything I've learned about encryption so far. The more I dig into cryptography, the more I realize how all these pieces fit together to create the security foundation we rely on every day.

Time to tackle the next room!
