---
title: "Diving into Cryptography - My First Steps"
date: 2025-08-19T10:43:28+05:30
draft: false
tags: ["cybersecurity", "cryptography", "encryption", "caesar-cipher", "vigenere-cipher", "symmetric-encryption", "asymmetric-encryption", "tryhackme", "learning-journey"]
---

So I've been diving deeper into cybersecurity lately, and one thing that kept coming up everywhere was cryptography. Honestly, I used to think crypto was just for hackers in movies or something super advanced that I'd tackle "someday." But turns out, this stuff is literally everywhere I look.

Started a TryHackMe room on crypto basics last week, and it got me thinking - how did we even get to the point where my WhatsApp messages are secure, but the internet itself isn't? That's a weird contradiction, right?

### The "aha" moment

Here's what blew my mind: every single time I log into my bank account, check my emails, or even just browse HTTPS sites, there's this invisible layer of math protecting me. Without it, anyone with a packet sniffer could basically read everything I'm doing online. That's terrifying when you think about it.

I mean, imagine if every text message you sent was like shouting across a crowded room - anyone could listen in. That's essentially what the internet would be like without encryption.

### Wait, I use this stuff daily?

This was my biggest realization this week. I'm not just learning about some abstract concept - I'm actually using cryptography constantly:

When I SSH into my home server, there's a whole handshake happening that I never paid attention to. My browser is constantly checking certificates when I visit websites. Even downloading files from GitHub involves hash verification (which I learned is also crypto!).

The crazy part? I've been taking all of this for granted. It just... works. But now I'm starting to understand the massive engineering effort behind making it "just work."

### Going back to the beginning

I decided to start from the very basics because, honestly, jumping straight into RSA and AES felt overwhelming. So I went way back - like, ancient Rome back.

The Caesar cipher is probably the simplest example I could wrap my head around. You just shift letters by a fixed number. So "HELLO" with a shift of 3 becomes "KHOOR". Obviously, this is laughably easy to break (there are only 25 possible shifts), but it helped me understand the basic idea: transform readable text into unreadable text using a secret (the shift number).

Then I found out about the Vigenère cipher, which uses a keyword instead of a single shift. It's like Caesar cipher on steroids. Still breakable, but it took centuries for people to figure out how.

Reading about how the Allies broke the Enigma machine during WWII was genuinely fascinating. These weren't just mathematical puzzles - breaking enemy encryption literally changed the course of history.

### Modern stuff is intimidating but fascinating

Now we're dealing with algorithms like AES (Advanced Encryption Standard) that would take longer than the age of the universe to brute force. That's... hard to even comprehend.

There are basically two main approaches I'm learning about:

**Symmetric encryption** is like having one key that both locks and unlocks the door. Super fast, but you need to somehow give that key to the other person securely. It's like trying to send someone a locked box along with the key - how do you keep the key safe during delivery?

**Asymmetric encryption** is this brilliant solution where you have two keys - one public, one private. You can literally post your public key on a billboard, and people can use it to send you encrypted messages that only your private key can decode. The math behind this still feels like magic to me.

Most real systems apparently use both together, which makes sense when you think about it.

### The math is... actually approachable?

I was dreading the mathematical side, but some of the basic operations are surprisingly simple. XOR (exclusive or) keeps showing up everywhere. It's just comparing two bits - if they're different, you get 1; if they're the same, you get 0. What's cool is that XORing something twice brings you back to the original: (A ⊕ B) ⊕ B = A.

I played around with this concept for a while, and it's actually the foundation of a lot of encryption schemes. Sometimes the simplest ideas are the most powerful.

Modulo arithmetic is another one that initially seemed intimidating but is actually just about remainders. 23 % 6 = 5 because 23 ÷ 6 = 3 remainder 5. This concept is used everywhere in crypto for wrapping numbers around.

### Where I'm headed next

Tomorrow I'm planning to tackle public key cryptography - specifically how asymmetric encryption actually works in practice. I keep hearing about RSA, digital signatures, and key exchange protocols, but I want to understand the mechanics behind them.

I'm particularly curious about:

- How does the whole "two keys" thing actually work mathematically?
- What's this Diffie-Hellman key exchange I keep seeing mentioned?
- How do SSH keys fit into all of this?
- What are digital signatures and how do they prove authenticity?

I have a feeling this is going to be another one of those topics that seems impossible at first but makes perfect sense once the right analogy clicks. The more I learn about cryptography, the more I realize how all these pieces fit together to create the security foundation we rely on every day.

It's wild to think that cryptography went from emperors sending secret military messages to being the foundation of our entire digital economy. And I'm just getting started understanding it all.

Anyway, that's my brain dump for today. Back to the labs!
