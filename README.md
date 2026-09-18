<p align="center">
  <img src="docs/icon.png" width="96" alt="">
</p>

<h1 align="center">spoti.pw</h1>

<p align="center">Spotify, in glass.</p>

<p align="center">
  <img src="https://img.shields.io/badge/iOS-000000?style=for-the-badge&logo=ios&logoColor=white" alt="iOS">
  <img src="https://img.shields.io/badge/Spotify-9.1.78-1ED760?style=for-the-badge&logo=spotify&logoColor=white" alt="Spotify 9.1.78">
  <img src="https://img.shields.io/badge/Objective--C-3A95E3?style=for-the-badge&logo=apple&logoColor=white" alt="Objective-C">
  <img src="https://img.shields.io/badge/GitHub_Actions-2671E5?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions">
  <img src="https://img.shields.io/badge/License-GPL_v3-blue?style=for-the-badge" alt="GPL-3.0">
</p>

<p align="center">
  <a href="https://spoti.pw">spoti.pw</a> ·
  <a href="#get-it">Get it</a> ·
  <a href="#build-it-yourself">Build it yourself</a> ·
  <a href="docs/tweaks.md">Hack on it</a>
</p>

<p align="center">
  <img src="docs/screenshots/now-playing.webp" width="16%" alt="Full screen player with lyrics">
  <img src="docs/screenshots/album.webp" width="16%" alt="Album">
  <img src="docs/screenshots/playlist.webp" width="16%" alt="Playlist">
  <img src="docs/screenshots/queue.webp" width="16%" alt="Queue">
  <img src="docs/screenshots/live-activity.webp" width="16%" alt="Live Activity on the lock screen">
  <img src="docs/screenshots/home.webp" width="16%" alt="Home">
</p>

A Theos tweak that rebuilds Spotify for iOS in Liquid Glass. A dylib, and a widget extension for
the Live Activity, injected into a decrypted IPA and signed with your own certificate, no jailbreak.
Settings are in Settings → Mod Settings, where one switch picks between the redesign and Spotify's
own look with its tweaks.

> [!IMPORTANT]
> Built and tested on **Spotify 9.1.78**, so use that version's IPA. The mod hooks Spotify's own
> classes, which change between releases: a newer or older Spotify may build fine and then lose parts
> of the redesign or crash.

## Get it

No IPA is distributed, here or on [spoti.pw](https://spoti.pw). Fork the repo and
[build it yourself](#build-it-yourself) from your own decrypted Spotify IPA; the GitHub workflow
needs no Mac. The Mod page tells you when a newer version is out.

The app keeps Spotify's bundle id, so it installs over the real Spotify.

### Signing it yourself

The bundle id you sign with has to match the App ID of your certificate. If it doesn't, the app
still installs and works, but tapping the player on the lock screen won't open it.

In Feather, copy the App ID from the certificate's tab into the **Identifier** field and leave
**PPQ protection** off, because it adds a random suffix to the bundle id. AltStore, SideStore and
Sideloadly get this right on their own.

If the ids don't match, the app tells you on first launch and gives you the bundle id to sign with,
ready to copy. The warning also stays in Mod Settings until you sign it again.

## Build it yourself

Bring a decrypted Spotify IPA. The result is an unsigned `Spotify-<version>-glass.ipa`, to sign with
SideStore, Feather or any certificate signer.

Use the IPA of **Spotify 9.1.78**, the version it is tested on.

### On GitHub, no Mac needed

Fork the repo, enable Actions, run the **Build IPA from your own Spotify IPA** workflow. It takes a
direct link to your decrypted `.ipa` and hands the built IPA back as a workflow artifact. The link is
masked in the log and the result stays in your fork.

### On a Mac

Theos in `~/theos` and Xcode with an iPhoneOS 26 or newer SDK (`xcode-select` it). An SDK in
`~/theos/sdks` alone builds too, but without the Live Activity, which needs Xcode's Swift toolchain.
Plus:

    brew install make ldid dpkg zsign ideviceinstaller libimobiledevice
    uv tool install "cyan @ git+https://github.com/asdfzxcvbn/pyzule-rw"

Put the decrypted `.ipa` in `ipa/`, then:

    make release    # out/Spotify-<version>-glass.ipa, ready to sign
    make install    # the same, signed with your certificate and pushed to the iPhone over USB

`make install` reads `SIGN_P12`, `SIGN_PROFILE` and `SIGN_P12_PASSWORD` from `.signing.env`; copy
`.signing.env.example` and fill it in. It signs under your profile's App ID, which is what keeps the
lock screen player working.

The first build spends a minute reading Spotify's flags out of your IPA, so the flag list matches the
Spotify you built from. `make flags` regenerates it.

## Star history

<a href="https://star-history.com/#skopevoj/spoti.pw&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=skopevoj/spoti.pw&type=Date&theme=dark">
    <img src="https://api.star-history.com/svg?repos=skopevoj/spoti.pw&type=Date" alt="Star history chart">
  </picture>
</a>

## Credits

[cyan](https://github.com/asdfzxcvbn/pyzule-rw) injects, [Theos](https://theos.dev) builds, and
[FLEX](https://github.com/FLEXTool/FLEX), as hopeless's AutoFLEX build in `vendor/`, is the inspector
the view trees are read through. The ad blocking and the Premium state are ported from
[EeveeSpotify Reincarnated](https://github.com/SideloadLabs/EeveeSpotifyReincarnated).

GPL-3.0. Not affiliated with Spotify.
