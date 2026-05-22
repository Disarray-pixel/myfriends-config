# PortalFriends Privacy Policy

**Version:** 1.0

---

## TL;DR

- **PortalFriends is a chat mod that does not require email or phone registration.** Instead of asking you to sign up the traditional way, we anchor your profile to your device so that one person cannot quietly create ten different accounts.
- **The clipboard inside the mod is also disabled** — not because we don't trust you, but because in a registration-free environment it is the easiest channel for mass spam and phishing. All of this is what allows us to keep the conversations comfortable for you.
- **No "raw" information about your computer** (hardware serial numbers, computer name, MAC address) **ever leaves your machine.** Only one-way cryptographic hashes (SHA-256) are sent to the server, and original values cannot be recovered from them.
- **We do not display third-party advertising**, we do not sell your data, and we do not share it with ad networks or analytics providers.
- **The service is non-commercial and free of charge**, with no subscriptions, premium tiers, or paid features.
- You may request deletion of your account and associated data at any time.

This document explains, in plain language, what we store and why. The legally binding wording is in the Terms of Service, Sections 12 and 13. In case of any conflict, this Privacy Policy takes precedence.

---

## 1. Why do we collect anything at all?

PortalFriends is a **registration-free mod**: you do not enter an email, choose a password, confirm a phone number, or sign in with social accounts. That is convenient for players, but it creates a serious problem: if nothing is tied to a real person, a single user can create ten accounts in a minute and flood the chat with spam, harassment, or phishing links.

To keep the experience comfortable, we still need some kind of "anchor" that is uniquely tied to a person. Most services use email or phone for that. We instead compute a technical "device fingerprint" — a short string that is derived stably from the hardware characteristics of your computer and does not change when you reinstall the mod or switch launchers.

This fingerprint allows us to:

- **distinguish users** from one another without email or password;
- **prevent return** of users previously banned by moderation, even if they pick a new nickname;
- **prevent a single user** from setting up ten accounts on the same computer to broadcast mass spam;
- **recognise your existing profile** when you come back, with no login/password needed.

If we did not do this, the mod would have to become a regular service with email + password + CAPTCHA + 2FA, which contradicts its core idea: "open Minecraft, open the portal, start chatting."

---

## 2. What data we collect

We aim to collect **only what is genuinely needed** to run the mod. The list below is exhaustive — we do not request or transmit anything beyond what is listed here.

### 2.1. Technical device identifiers

When the mod first starts, it reads several technical characteristics from your computer. **All of them are standard and are not personal data.** We disclose explicitly which ones:

**Primary signals:**
- **SMBIOS UUID** — the standard motherboard identifier.
- **MachineGuid** (on Windows) / **`/etc/machine-id`** (on Linux) / **IOPlatformUUID** (on macOS) — the operating system installation identifier.
- **Motherboard serial number.**
- **BIOS serial number.**

**Secondary signals (if available):**
- **System disk serial number.**
- **CPU model name** (for example, "Intel Core i5-12400F").

**What happens to this data next:**

1. On your computer, each value is immediately passed through a one-way cryptographic function **SHA-256 with a fixed salt**. The output is a short string like `Ab3k9Xq...`, from which the original value **cannot** be recovered (this is a fundamental mathematical property of cryptographic hash functions).
2. Only these **hashes** are sent to the server. The raw serials, UUIDs, and model names **never leave your computer**.
3. On the server, the hashes are used strictly for two purposes: (a) to recognise that a new mod installation belongs to an already registered user, and (b) to detect attempts to evade a moderation ban.

**Based on these hashes, the mod computes a short `installId` identifier** (derived from the primary signals via the standard HKDF-SHA256 cryptographic scheme). That identifier is your "anchor" in the system.

### 2.2. Cryptographic device key

When the mod first starts, it also generates a pair of cryptographic keys (Ed25519) on your computer. **The private key stays with you**, securely stored in your operating system's protected secret store:

- **Windows** — through DPAPI (Data Protection API), the standard Windows secret-protection mechanism;
- **macOS** — through Keychain (the system password and certificate manager);
- **Linux** — through libsecret (gnome-keyring / KWallet — the system password manager).

Only the **public key** (`devicePublicKey`) is transmitted to the server. It is used to verify that requests really come from your device. The public key cannot be used to decrypt your messages or to impersonate you.

### 2.3. Profile data

After registration, your profile stores:

- **Nickname with a numeric tag** of the form `Name#1234`, generated by the service. The name itself is what you enter at first launch.
- **Minecraft in-game nickname** (`mcName`) — a value you self-declare; **it is not verified** and is only used to display in your profile card.
- **Self-declared client type** (premium / cracked) — you state it yourself during onboarding; we do not perform any Mojang / Microsoft verification.
- **Account creation date.**
- **Last activity timestamp** (needed to show the "Online / Offline" status on your profile card).
- **Your friends list** and pending friend requests.
- **List of other Minecraft mods installed in your `mods/` folder** — used solely for the "same modpack" feature on profile cards. We read mod names and versions once on login and save them on your profile so that we can show you players running the same set of mods, letting you play together without conflicts. The contents of the jar files themselves (classes, resources) are not parsed or copied.
- **Flags** "visible in player catalogue" and "receive push notifications".

### 2.4. Content data (the things you type)

- **Texts of your private messages** to other users.
- **Texts of complaints** you file against other users.
- **Your "useful / not useful" votes** on news feed posts (no free-form comment).

### 2.5. Technical network metadata

- **IP address** is logged **only in security audit logs** to defend against automated attacks (rate-limit, anti-bot). Retention of IPs in these logs is **no more than 30 days**, after which entries are irreversibly deleted.
- **User-Agent and request timestamps** — standard information recorded by HTTP servers.

---

## 3. What we DO NOT collect

We think it is important to spell out explicitly what we do **not** touch, so you do not have to wonder "do they know that too?":

- **MAC address** of your network interfaces.
- **Device hostname** and **operating-system username.**
- **List of running processes** or **installed programs.**
- **Clipboard contents.** Moreover, the mod intentionally **disables the clipboard in input fields** — this is a defence against mass-pasting spam and phishing links.
- **Minecraft servers** you connect to.
- **In-game coordinates** (where your character is in the world).
- **Voice / microphone / camera** — the mod has no voice or video functionality whatsoever.
- **Geolocation** — we do not request OS-level location, we do not infer country from IP for marketing purposes, and we do not serve region-specific content.

There are **no** third-party analytics SDKs, trackers, ad networks, or fingerprinting libraries (such as Google Analytics, Yandex.Metrika, Sentry, FingerprintJS) in the mod.

---

## 4. Where and how we store data

- **The server-side** runs on a single dedicated server. Communication between the mod and the server is **encrypted** with modern TLS — the same technology that protects banking sites in your browser.
- **The database** lives on the same server; only the Service administrators have access to it.
- **Database backups** are stored in encrypted form; the backup decryption key is not shared with the hosting provider.
- **Protected secrets on your device** (private key, session tokens) are stored in the **operating-system secret store** (DPAPI / Keychain / libsecret), not in a plain file. This means even another user on the same physical computer, logged into a different OS account, cannot read your keys.

---

## 5. To whom we transfer data

**To no one, except in the cases listed below:**

- **The server hosting provider** — to the unavoidable technical extent (traffic, basic infrastructure log files). The provider has no access to the database contents or to decrypted application traffic.
- **CDN / DDoS protection** — when needed, we may place Cloudflare or a similar service in front of the server. In that case, TLS traffic is briefly decrypted at the CDN edge; the CDN does not retain request contents.
- **Government authorities** — only when a properly issued legal request from the state under whose jurisdiction the Operator resides is received, and only to the extent required by that request. We do **not** share data voluntarily in response to informal inquiries.

**We never share data with:**

- Advertising networks, marketing agencies, or analytics companies — **never.**
- Data brokers, aggregators, or database sellers — **never.**
- Other Minecraft mods, launchers, or server plugins — **never** (the mod is sandboxed and does not "expose" your data to any third-party code).

**We do not sell your data.** The service is non-commercial and free; there is simply no revenue source that would create an incentive for such sales.

---

## 6. Retention periods

- **Profile and account data** — for as long as your account exists.
- **Messages** — until you delete them yourself, or until the account is deleted.
- **IP addresses in rate-limit logs** — no more than **30 days.**
- **Technical security audit logs** — no more than **180 days** (needed to investigate incidents).
- **Deleted accounts** — anonymised within **30 days** of the deletion request being confirmed; messages already sent to other users remain in their inboxes with a "deleted account" marker.

---

## 7. Your rights

You may, at any time:

- **Request information** about what we store specifically about you (via the mod's "Help / Support" section).
- **Request deletion** of your account and all associated data (TOS Section 15). Deletion is performed within 30 days of request confirmation.
- **Correct inaccurate data** in your profile (display name, mcName, client type, visibility flags) — via the mod's interface.
- **Hide yourself from the public player catalogue** — by toggling the corresponding flag in profile settings.

Requests for processing restriction, data portability, or consent withdrawal are submitted through the same "Help / Support" channel. Response time is up to 30 calendar days.

---

## 8. Security

- The mod ↔ server channel is protected by modern **TLS** (HTTPS).
- Session tokens are **short-lived**; long-term authentication uses **cryptographic request signing** with a private key that never leaves your device (the OS secret store prevents its theft).
- The server applies multi-tier **rate-limiting** so that automated attacks cannot exhaust service resources.
- Access to the administrative side of the Service is protected by **two-factor authentication.**

Despite these measures, we say honestly: **absolute security is impossible.** If you discover a vulnerability in the Service — please do not exploit it and do not disclose it until it is fixed; write to us via the mod's interface. We are grateful to security researchers and try to respond within reasonable timeframes.

---

## 9. User age

The Service is intended for users **at least 13 years old.** If you are younger than 13, please do not use the mod and ask your parents or guardians to move you to a service designed for younger children.

If we learn that an account is being used by a child under 13, we reserve the right to delete that account.

---

## 10. Policy updates

We may update this policy over time — for example, if we refine the list of collected data or change hosting provider. **Material changes** (those directly affecting your data — the set of collected fields, the parties we share with, retention periods) are accompanied by:

- updating the version number and date at the top of this document;
- a news-feed announcement in the mod **at least 14 calendar days before** the change takes effect;
- a re-consent prompt in the mod's interface at the next launch.

Non-material technical edits (typo fixes, clarifying wording without changing meaning) may be made without specific notice.

---

## 11. Contacts

The Service **does not have** an identifiable legal entity, physical address, or publicly listed administrator. All inquiries about processing of your data go through the mod's "Help / Support" section. Response time is up to 30 calendar days.

---

**Effective date:** 21 May 2026.

**Related documents:** [PortalFriends Terms of Service](tos_en.md), Section 12 ("Collection and processing of personal data") and Section 13 ("Service security").
