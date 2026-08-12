# fk-road-dist

Update feed and release artifacts for **fk-road** (Palenque Agent, macOS).

This repository holds **only** the Sparkle appcast and signed release builds.
No source lives here — that is in the private `fk-remote` repo.

- `appcast.xml` — the Sparkle feed the app polls. Served raw over HTTPS:
  `https://raw.githubusercontent.com/farazkhalid/fk-road-dist/main/appcast.xml`
- `releases/` — notarized, stapled `.zip` builds referenced by the appcast.

The repo is **public on purpose**: Sparkle fetches the appcast unauthenticated,
so a private repo would break updates for every installed copy.

Every build is Developer ID signed, notarized by Apple, and additionally signed
with an EdDSA key whose public half is pinned in the app's `Info.plist`
(`SUPublicEDKey`). The private half never leaves the release machine's keychain.
A tampered build fails that check even if it were somehow served from here.

Releases are cut with `scripts/release-mac.sh` in the source repo — do not edit
`appcast.xml` by hand; it is generated.
