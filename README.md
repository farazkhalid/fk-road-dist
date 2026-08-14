# fk-road-dist

Update feed and release artifacts for **fk-road** (Palenque Agent, macOS).

## Install on a new Mac

**[Download fk-road-setup.zip](https://github.com/farazkhalid/fk-road-dist/releases/latest/download/fk-road-setup.zip)**
— unzip it and run `Setup.command`.

That link always points at the newest release, so it is safe to write down or
pass on. The bundle contains the notarized app, the setup script and a README;
you need nothing else installed — no checkout, no Homebrew, no command line.

You also need a **join key**, which is deliberately *not* in the bundle: it is
what lets someone create an account on the mesh server. Ask Faraz for it
separately.

Already running the app? Do nothing — it updates itself through the feed below,
or immediately via the menu bar's "Check for Updates…".

## What is in here

This repository holds **only** the Sparkle appcast and signed release builds.
No source lives here — that is in the private `fk-remote` repo.

- `appcast.xml` — the Sparkle feed the app polls. Served raw over HTTPS:
  `https://raw.githubusercontent.com/farazkhalid/fk-road-dist/main/appcast.xml`
- `releases/` — notarized, stapled `.zip` builds referenced by the appcast.
  Only app builds belong here: `generate_appcast` signs every zip it finds in
  this directory, so anything else dropped in would be advertised to Sparkle as
  an app update. The setup bundle is a GitHub *release asset* for that reason.

The repo is **public on purpose**: Sparkle fetches the appcast unauthenticated,
so a private repo would break updates for every installed copy.

Every build is Developer ID signed, notarized by Apple, and additionally signed
with an EdDSA key whose public half is pinned in the app's `Info.plist`
(`SUPublicEDKey`). The private half never leaves the release machine's keychain.
A tampered build fails that check even if it were somehow served from here.

Releases are cut with `scripts/release-mac.sh` in the source repo — do not edit
`appcast.xml` by hand; it is generated.
