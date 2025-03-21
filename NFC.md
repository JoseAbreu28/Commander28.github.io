## NFC Relay Attacks

In today's security landscape, organizations increasingly implement _Access Control List_ (ACL) solutions based on ISO-14443-A or ISO-14443-B standards. While these systems provide efficient access control mechanisms, they are not immune to sophisticated attacks. One such attack vector that caught my attention during a recent Red Team engagement was the NFC relay attack, reminiscent of the "[Keyless Entry System Attacks](https://archive.org/details/youtube-rhm1TiFJc7s)" presentation I attended.
At the time, I attempted to reproduce the relay attack using an Arduino-based solution but left the project pending. However, after executing a physical Red Team assessment, the concept resurfaced. In my research for practical implementations, I discovered a fantastic open-source project on [GitHub by burja8x](https://github.com/burja8x/relay) that simplifies the relay attack process. This project leverages Proxmark devices, which are relatively affordable and easy to configure.

## Understanding NFC Relay Attacks
_Near Field Communication_ (NFC) relay attacks exploit the wireless nature of NFC communication, allowing an attacker to artificially extend the range between an NFC reader and a legitimate tag. Like any relay attack, this method relies on two proxies that retransmit the connection between the involved parties, tricking the system into believing that authentication is happening normally.
From a security perspective, this attack poses a significant threat because it enables attackers to bypass authentication mechanisms that rely on physical proximity. One of the most common examples of this attack is automobile theft. Attackers can capture the signal from a key fob inside a victim’s home and relay it to a second device near the target vehicle, tricking the car into unlocking and starting without requiring the actual key. Another scenario involves ATM fraud, where attackers relay the authentication data from a contactless payment card to an ATM or a point-of-sale terminal, allowing unauthorized transactions without direct physical access to the card. Corporate access control systems are also vulnerable, as attackers can relay NFC-based authentication credentials from an employee’s badge to gain unauthorized entry into restricted areas.
The core vulnerability exploited in NFC relay attacks is the system's reliance on the short-range nature of NFC communication. Since NFC does not inherently verify the distance between devices, attackers can manipulate the communication channel to their advantage, making it crucial for organizations and individuals to implement additional security measures such as multi-factor authentication and anomaly detection systems.

![NFC Realy Attack](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjgne3B23DZwVZFP3RD9ZRQG2rpkyxkdBToENUo65N020x9bE9nV-W4mCo8ueScyhmjsbG-u0buW3RqQmujFx490xeodLN4Oyj4ws1Xiazzi89zG1ekitzCoyeBbXrk1JvAvYVAWmus-qw/s728-rw-e365/credit-card-hacking.jpg)

## Overview of the burja8x GitHub Relay Project
The implementation of this project was quite straigth forward, I only needed to configure the Proxmark code for the modified version, and to do this I followed the steps described in the project.

## How It Works  

1. **Mole selects a tag:** It captures essential data such as UID, ATQA, SAK, and ATS, then transmits it to the proxy device.  
2. **Proxy emulation:** The proxy device reads the received data and begins the emulation process, fully imitating the tag.  
3. **Relay communication:** The tool relays raw commands between the two devices.  
4. **Timing extensions:** The tool utilizes additional commands (WTX) to extend the allowable response time, enhancing the attack's reliability.  

![NFC-data](https://github.com/user-attachments/assets/41daeda7-5d6f-444f-9aed-94bd1f57628d)

## My Implementation
To ensure precision, I replicated the setup as detailed in the GitHub repository.
A Raspberry Pi 4, Raspberry Pi Zero, Proxmark 3 Easy and Proxmark 3 RDV2 were used for this implementation.However, for future improvements, I plan to integrate the [DL533N XL](https://lab401.com/products/long-range-rfid-reader-writer-dl533n-xl) module, which allows ISO/IEC 14443A/B reading at distances of up to 18 cm. 

[![POC](http://i.ytimg.com/vi/CvHqo6z4kx0/hqdefault.jpg)](https://www.youtube.com/watch?v=CvHqo6z4kx0)

## The Strength of NFC Relay Attacks  

One of the most significant advantages of this attack is its stealthiness. Since it merely extends the ISO-14443-A communication link, it circumvents many security measures implemented in NFC-based systems, including:  

- **Encryption mechanisms:** Since the original encrypted communication is relayed without decryption, it remains undetectable by standard security protocols.  
- **Mutual authentication:** The attack does not require breaking the authentication process; it merely extends it.  
- **Distance limitations:** Traditional NFC security relies on close-proximity interactions, which this attack effectively bypasses.  

## Mitigation Strategies  

Currently, the only effective mitigation against NFC relay attacks is the use of Faraday cages or signal-blocking pouches. These solutions prevent the unauthorized extension of NFC signals, ensuring that communication remains within the intended physical range.  

## Conclusion  

As NFC technology continues to be widely adopted in security-critical applications, the threat of relay attacks remains a significant concern. While this attack vector is well-known in cybersecurity circles, the ease of implementation using open-source tools and affordable hardware makes it more accessible than ever. Security professionals and organizations must remain vigilant, adopting robust countermeasures to mitigate the risks associated with NFC relay attacks.


