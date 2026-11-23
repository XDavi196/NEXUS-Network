# NEXUS Network

<p align="center">
  <img src="assets/nexus-network-logo.png" alt="NEXUS Network logo" width="180" />
</p>

**An independent, community-made fork of [ZNeko Launcher](https://github.com/zneko-org/zneko-launcher) that adds a Discord-powered _NEXUS Network_ tab.**

> ## ⚠️ Important: this is an independent fork
>
> This project is **not** affiliated with, endorsed by, sponsored by, or maintained by the ZNeko team or the original ZNeko developer(s). **The ZNeko team and developer do not contribute to this fork**, do not review it, and are **not responsible** for it in any way. They do not provide support for it.
>
> - **Do not** report bugs, request features, or ask for help about this fork in the official ZNeko channels. Anything that goes wrong here is caused by *this* fork, not by them.
> - **Do** open an [issue in this repository](../../issues) instead.
> - All credit for ZNeko itself belongs to its authors. Everything good about the launcher is theirs; any mistakes introduced by the changes listed below are mine.
>
> The same applies to **iiSU** (design inspiration only) and **Discord** (a trademark of Discord Inc.). This project has no connection to them beyond using Discord's public developer platform.

---

## Table of contents

1. [What is this?](#what-is-this)
2. [Status](#status)
3. [Features](#features)
4. [What is different from upstream ZNeko](#what-is-different-from-upstream-zneko)
5. [Installation](#installation)
6. [Using NEXUS Network](#using-nexus-network)
7. [Discord setup (for developers)](#discord-setup-for-developers)
8. [Privacy and security](#privacy-and-security)
9. [Known limitations](#known-limitations)
10. [How this fork is built](#how-this-fork-is-built)
11. [Reporting problems](#reporting-problems)
12. [Credits and licenses](#credits-and-licenses)
13. [Disclaimer and takedown policy](#disclaimer-and-takedown-policy)

---

## What is this?

[ZNeko Launcher](https://github.com/zneko-org/zneko-launcher) is a game-library frontend for Android (shortcut-style home screen, themes, RetroAchievements integration, controller-friendly navigation and more). **NEXUS Network** is a fork that adds a new section to the launcher's bottom bar, a Wi-Fi icon, that brings Discord into the launcher: sign in with Discord, keep a list of contacts, and (when the optional native module is installed) see which friends are online, what they are playing, and chat by direct message.

The goal is to keep everything looking and behaving like the original: the new tab reuses ZNeko's own card design, entrance animation, focus highlight, theme colors, text-input dialog, footer hints and gamepad navigation.

## Status

**Experimental.** This is a hobby project, built and tested by one person on one device.

| Component | State |
|---|---|
| NEXUS Network tab, ZNeko-style design, gamepad navigation, light/dark theme, accent color, English/Portuguese | ✅ Working |
| Sign in with Discord (OAuth2 with PKCE) and Discord avatar as the profile picture | ✅ Working |
| Contacts (save a friend's Discord ID, open the chat in Discord) | ✅ Working |
| Online friends, "now playing" details and direct messages through the **Discord Social SDK** | 🛠️ In development, not yet released. The UI is finished; the native module still has to be compiled and tested on a real device |
| Custom gray launcher icon | ✅ Included |

Features marked "in development" are **not** part of the current release builds. They will be announced in the release notes when they ship.

## Features

- **New "NEXUS Network" tab** in the section bar, with its own Wi-Fi icon:
  - normal (outline) icon when not selected,
  - colored icon when selected,
  - layered 3D icon in the section-switcher, like the other sections.
- **Discord sign-in** using OAuth2 with PKCE (no client secret is stored or needed). The Discord app is used when it is installed.
- **Discord avatar** replaces the profile picture in the header after you connect, the same way ZNeko does it for RetroAchievements. A custom profile picture chosen in ZNeko's own settings is respected and takes priority.
- **Contacts**: store friends' Discord IDs with a nickname and jump straight into the conversation in the Discord app.
- **Friends and DMs (Social SDK build only)**: online friends, the game each one is playing (title, details, platform/state, elapsed time and artwork when Discord provides it) and direct messages.
- **Follows ZNeko's settings**: light/dark theme, accent color, and language (English / Português).
- **Full controller support**: D-pad/stick to move, **A** to select, **B** to go back inside the tab, using ZNeko's own focus styling and footer hints.
- **Gray launcher icon** (the original is orange) so the fork is easy to tell apart from upstream on your home screen.

## What is different from upstream ZNeko

This fork ships a modified build of the ZNeko app. Compared with the version it is based on, it adds or changes:

1. A new `nexus` section (tab, icons, translations, page).
2. Discord sign-in code (OAuth2 + PKCE) and a deep link, `nexus://auth`, so the browser/Discord app can return to the launcher.
3. An optional deep link for the Discord Social SDK (`discord-<application id>://authorize/callback`) and an optional native plugin (only in builds that include the SDK module).
4. Avatar hook: the profile picture can come from Discord.
5. Gamepad handling so the new tab behaves like the other full-screen tabs.
6. A new launcher icon (gray background; same fox logo).

Nothing else is intentionally changed. In particular, the ZNeko app's own data and settings are left alone.

> **Different package signature.** Because this build is re-signed with a different key, Android treats it as a different publisher. You must uninstall the original ZNeko before installing it, and you cannot update one to the other in place. Back up whatever you need first.

## Installation

**Requirements:** Android 11 or newer (tested on Android 11), a 64-bit ARM device (arm64). Allow "Install unknown apps" for your browser or file manager.

1. Go to the [**Releases**](../../releases) page and download the latest `NEXUS-ZNeko.apk`.
2. **Uninstall the original ZNeko Launcher** (see the signature note above).
3. Install the downloaded APK and open it.
4. Open the **NEXUS Network** tab (Wi-Fi icon) in the bottom bar and choose **Sign in with Discord**.

Updating this fork is done by installing a newer release of *this* fork on top of the previous one (they share the same signing key).

## Using NEXUS Network

- **Sign in:** tap/select **Sign in with Discord**. Authorize the app in the Discord app or browser. You return to the launcher automatically and your Discord avatar becomes your profile picture.
- **Contacts:** *Contacts → Add contact*, then enter the friend's Discord **user ID** and a nickname. Select a contact, then **Open chat in Discord**.
  - To find an ID: Discord → *Settings → Advanced → Developer Mode*, then long-press the user and choose *Copy User ID*.
- **Friends and DMs (when the Social SDK module is installed):** *Online friends* lists who is online and what they are playing; select a friend to see the activity card and write a message. *Conversations (DMs)* lists your recent chats.
- **Sign out:** *Sign out* at the bottom of the tab. It clears the stored session and restores your previous profile picture.

Controller: **D-pad / stick** moves the focus, **A** selects, **B** goes back one screen inside the tab.

## Discord setup (for developers)

You only need this if you build or host your own variant with your own Discord application. Users of the release APK do **not** need to do anything.

In the [Discord Developer Portal](https://discord.com/developers/applications), for your application:

1. **OAuth2 → Public Client:** enable it (the app never holds a client secret).
2. **OAuth2 → Redirects:** add both URIs exactly:
   - `nexus://auth`
   - `discord-<your application id>:/authorize/callback` (only needed for the Social SDK build)
3. Scopes requested: `identify guilds` for the plain sign-in, and the SDK's default communication scopes for the Social SDK build.

No bot token and no server are used. Do **not** put any secret in the app.

## Privacy and security

- Everything runs **on your device**. There is **no server operated by this project**, no analytics, no telemetry and no ads.
- Your Discord access/refresh tokens, your contacts list and your language/profile preferences are stored **locally** in the app's own storage. Signing out removes the session; uninstalling the app removes everything.
- The app talks only to Discord's own services (and Discord's CDN for avatars/artwork).
- Only the minimum permissions are requested. Review the permissions Android shows when you install.
- This is an unofficial modification. Only install it from this repository's Releases page and check that the APK you installed is the one you downloaded. If you do not trust a build, do not install it.
- If you find a security problem, please open a private security advisory (or an issue without sensitive details) in this repository, **not** with the ZNeko team.

## Known limitations

- Online friends, "now playing" and DMs require the Discord Social SDK module and are **not available in the current release**.
- What a friend appears to be playing depends on their own Discord settings ("Share your activity") and on the game or app reporting its status to Discord. Many mobile games and emulators do not.
- The Discord Social SDK only exposes friends that are on your Discord friends list, and its behavior is controlled by Discord. Features can change or stop working if Discord changes its platform.
- If you chose to keep a custom profile picture in ZNeko, the Discord avatar will not replace it.
- Because the original ZNeko may change internally at any time, this fork must be rebuilt for each new ZNeko version, so updates here can lag behind upstream.

## How this fork is built

The original ZNeko source code is not part of this repository (the upstream repository publishes documentation and releases only), so this fork is produced by **patching the official ZNeko release build**:

- The web layer (the part that draws the interface) is modified to add the NEXUS Network tab and the Discord sign-in logic.
- The Android manifest gets the deep links needed to return from Discord.
- Icons are replaced.
- The package is re-signed with this fork's own key.

The optional Social SDK module is a separate small native project (C++ and Java) written for this fork on top of Discord's **official** Discord Social SDK. It is kept in a **private** repository because the SDK files are distributed by Discord under their own license and must not be redistributed publicly. Only the compiled result is included in releases that ship it. This fork does **not** use code from iiSU or from any other launcher.

This repository itself contains documentation and release information only.

## Reporting problems

- Use this repository's [**Issues**](../../issues).
- Please include: the release you installed, your Android version and device, what you did, what you expected and what happened (a screen recording or screenshot helps a lot).
- Remember: the ZNeko team and developer **do not** handle issues for this fork.

## Credits and licenses

- **ZNeko Launcher**: the launcher this fork is based on. All rights to ZNeko belong to its authors. See the [upstream repository](https://github.com/zneko-org/zneko-launcher) for its license and terms; they continue to apply to everything that originates from ZNeko (sounds, models, design, code, services). This fork does not claim ownership of any of it.
- **iiSU**: visual inspiration for the library/section-bar style. No iiSU code or assets are used.
- **Discord**: Discord and the Discord Social SDK are products and trademarks of Discord Inc. Use of Discord's platform is subject to [Discord's terms and developer policies](https://discord.com/developers/docs/policies-and-agreements/developer-terms-of-service).
- **Lucide Icons** ([ISC license](https://lucide.dev/license)): base shapes for the Wi-Fi icon and the small UI icons.
- Fork by [@XDavi196](https://github.com/XDavi196).

"ZNeko", "iiSU" and "Discord" and their logos are the property of their respective owners. They are used here only to describe compatibility and credit the original work.

## Disclaimer and takedown policy

This software is provided **"as is"**, without warranty of any kind. Use it at your own risk. It is a fan-made, non-commercial project; it is free and will stay free.

If the ZNeko team, the original developer, iiSU, Discord, or any other rights holder asks for this fork to be changed or removed, the request will be honored. Please open an issue or contact [@XDavi196](https://github.com/XDavi196) through GitHub.

