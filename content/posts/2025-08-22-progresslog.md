---
title: "John the Ripper: My First Real Password Cracking Experience"
date: 2025-08-22T10:13:08+05:30
draft: false
tags: ["cybersecurity", "john-the-ripper", "password-cracking", "hash-cracking", "offensive-security", "tryhackme", "penetration-testing"]
---

![John the Ripper](/images/John-The-ripper.jpeg)

Well, I promised yesterday I'd dive into John the Ripper, and holy cow, this tool is both fascinating and terrifying. After spending the last few days learning about hashing theory, getting hands-on with an actual password cracking tool really drove home how vulnerable weak passwords actually are.

I went through the TryHackMe room on John the Ripper today, and it was like getting a masterclass in offensive security. The scary part? This stuff actually works, and it works fast.

### Starting with the basics (and feeling overwhelmed)

The room started by explaining what John actually does, and it's beautifully simple in concept: since you can't reverse a hash, John just hashes a ton of password guesses and compares them to your target hash. When they match - boom, password cracked.

The basic syntax looked straightforward enough:
```
john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

But man, there are so many options and modes. I felt like I was looking at a Swiss Army knife with 50 different tools.

### Hash identification became my new puzzle

One thing that caught me off guard was how picky John can be about hash formats. The room showed me `hash-identifier`, this Python tool that tries to guess what type of hash you're dealing with.

My first real test came when I had to crack this mysterious hash: `c5a60cc6bbba781c601c5402755ae1044bbf45b78d1183cbf2ca1c865b6c792cf3c6b87791344986c8a832a0f9ca8d0b4afd3d9421a149d57075e1b4e93f90bf`

![Reading the Hash](/images/Hashes-read-hash.png)

Looking at that massive string, I had absolutely no idea what type of hash it was. That's where the `hash-id.py` tool became my lifesaver.

![Identifying the Hash Type](/images/Hashes-Found-hashtype.png)

I fed it the hash and it told me it was likely SHA-512 or Whirlpool. Not exactly confidence-inspiring when it gives you multiple options, but at least it narrowed it down. The tool would give me a list like "probably this, maybe that, could be this other thing" - definitely not the definitive answer I was hoping for.

I decided to try Whirlpool first, and that's when the magic happened:

![Cracking the Password](/images/Hashes-Cracked-Password.png)

And boom! John cracked it in what felt like seconds. The password was "colossal" - which honestly made me chuckle. All that cryptographic complexity defeated by a single dictionary word from the rockyou.txt file.

### The moment when theory met reality

Remember that `rockyou.txt` file I mentioned from the 2009 breach? Well, it turns out that file is John's bread and butter. 14+ million real passwords that people actually used. Watching John churn through thousands of attempts per second was both mesmerizing and horrifying.

That first successful crack really hit me - if your password is in that wordlist, you're toast. The speed was what shocked me most. All those theoretical discussions about hash security, and here I was watching it get defeated in seconds.

### Windows and Linux password adventures

The room walked through cracking actual system passwords, and this is where things got really interesting. Learning about NTLM hashes from Windows SAM databases felt like peeking behind the curtain of how Windows actually works.

![Cracking a Windows Password](/images/cracked-password-windows.png)

The Windows NTLM hash was a completely different beast from what I'd been working with. Using `john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt`, I watched it work through the rockyou wordlist. When it cracked, the password turned out to be "mushroom" - another simple dictionary word that probably took John milliseconds to find.

What struck me was how fast these "secure" password hashes fell to a basic dictionary attack. Different operating system, different hashing algorithm, same result.

The Linux side with `/etc/shadow` was even more fascinating. That `unshadow` tool that combines `/etc/passwd` and `/etc/shadow` files? Genius. I never realized why both files exist, but now it makes sense - separation of public info (usernames, home directories) from sensitive stuff (password hashes).

![Unshadowing and Cracking the Password](/images/etcshadows-unshadowing-and-cracking-the-password.png)

This was the most realistic scenario - cracking an actual Linux system password. After running `unshadow`, I had a properly formatted file that John could work with. The `$6$` prefix told me this was SHA512crypt - one of the stronger hashing algorithms used by modern Linux systems.

I thought this one might take longer to crack since it's considered very secure, but I was completely wrong. The root password? "123456". I actually laughed out loud when I saw that. Here's a system using strong cryptographic hashing (SHA512crypt), but the user chose literally one of the most common passwords ever.

### Single crack mode blew my mind

This is where John gets really clever. Instead of just trying dictionary words, it can use the username to generate password guesses. So for a user named "markus", it might try "markus1", "Markus123", "markus!", etc.

The concept of word mangling is brilliant and terrifying. John takes whatever information it has about the target (username, full name from GECOS field) and systematically modifies it following common password patterns.

I tested this on a hash where the password was literally the username with "123!" appended, and John cracked it in single mode faster than wordlist mode. That's the scary part - people really do create predictable passwords based on their usernames.

### Custom rules: When you know too much about human psychology

The custom rules section made me realize how predictable we are as humans. The room showed how to create rules that match common password complexity requirements - capital letter at the start, number and symbol at the end.

The syntax looked like regex had a baby with some other markup language:
```
[List.Rules:PoloPassword]
cAz"[0-9][!£$%@]"
```

That rule basically says "capitalize first letter, then append a number 0-9, then append one of those symbols." And honestly? That probably covers like 60% of "complex" passwords that meet corporate requirements.

I tried creating my own rule based on password patterns I've seen at previous jobs, and it was depressingly effective when I tested it on sample hashes.

### Beyond text passwords: Files, archives, and keys

The second half of the room opened up a whole new world. John can crack ZIP file passwords? RAR archives? SSH private key passphrases? Mind blown.

The `zip2john` and `rar2john` tools convert these protected files into formats John can work with. I downloaded a password-protected ZIP file and walked through the process:

1. `zip2john protected.zip > zip_hash.txt`
2. `john --wordlist=rockyou.txt zip_hash.txt`
3. Watch John work its magic

Seeing it actually recover the ZIP password was surreal. All those times I've forgotten archive passwords - turns out there was a solution all along.

The SSH key cracking with `ssh2john` was particularly interesting since I use SSH keys daily. It's a good reminder to always use strong passphrases on private keys, even if they never leave your machine.

### What these practical exercises taught me

Seeing these hashes get cracked so easily was genuinely eye-opening:

1. **Hash strength doesn't matter if the password is weak** - SHA512crypt is considered very secure, but "123456" is still "123456" no matter how you hash it.

2. **The rockyou.txt wordlist is terrifyingly effective** - Every single password I cracked was in that list of real passwords from the 2009 breach. It's a sobering reminder of how predictable humans are.

3. **Speed matters** - Even in my slow VM, John was trying thousands of passwords per second. On dedicated hardware with GPU acceleration, this would be orders of magnitude faster.

4. **Different hash types, same result** - Whether it was Whirlpool, NTLM, or SHA512crypt, weak passwords fell just as fast.

### The ethical weight of this knowledge

Throughout the entire room, I kept thinking about the ethical implications. These tools are incredibly powerful, and in the wrong hands, they could do real damage. But understanding how they work is crucial for defense.

Every time John cracked a hash in seconds, it reinforced why proper password policies matter. It's not just theoretical anymore - I've seen firsthand how fast weak passwords fall.

The most important lesson? Technical security controls are only as strong as the human choices behind them. You can have the most sophisticated hashing algorithm in the world, but if users choose "password123", you might as well be storing passwords in plain text.

### Performance observations and limitations

Running everything in my VM was... slow. John kept mentioning GPU acceleration and how much faster it could be, but VMs don't play well with GPU passthrough. I'm definitely adding "run John on bare metal" to my learning todo list.

Even on my limited setup, watching John try thousands of password combinations per second was impressive. I can only imagine what it's like on a proper cracking rig with multiple high-end GPUs.

### What really stuck with me

This wasn't just about learning a tool - it was about understanding human behavior and how it relates to security. The single crack mode exploiting username patterns, the custom rules matching password complexity requirements, even the ZIP file cracking - it's all based on predictable human choices.

The most valuable lesson? Security isn't just about strong algorithms and good implementation. It's about understanding that humans are often the weakest link, and tools like John are really good at exploiting human predictability.

### Practical takeaways for my future

I'm definitely going to:
- Test any password policies I create against John's rule sets
- Always use truly random passwords instead of following predictable patterns
- Understand that "complexity requirements" alone don't create strong passwords
- Remember that any password-protected file might be crackable given enough time

This hands-on experience really reinforced why password policies and user education are so critical in cybersecurity. It's not enough to implement strong technical controls - you need to help users make better choices too.
